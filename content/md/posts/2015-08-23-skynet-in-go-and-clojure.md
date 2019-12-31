{:title "Skynet in Go and Clojure"
:layout :post
:tags ["practice" "clojure" "go"]}

### Codingame

[Codingame](https://www.codingame.com/) is a platform (one of many, but the one well done, with a great implementation) where you can practice your algorithmic and problem solving skills on a nice set of selected problems. In this post I will make an analysis of the same problem solved in 2 different languages - Go and Clojure. Emphasis on the language constructs rather than the algorithm itself (which is a simple breadth-first graph search).

Why Go and Clojure? These are 2 languages in the moment I am more interested, opposed to day to day Java development. And they have unique set of features which at least good to become familiar with.

### Puzzle

The puzzle I will talk about is the following one [Skynet](https://www.codingame.com/ide/1838509919700ece68cf28e8c639f9d8b7fa87b). I got a permission from the Codingame team to publish the solution of this puzzle online. Although the main idea is to focus on the languages rather than algorithm. Below there are annotated solution in Go and annotated solution in Clojure. Worth to mention that I've solved the problem with Go first, and then with Clojure later, so it is reflected in the annotations for each of the solution.

### Algorithm in plain English

Before I dive into the code, let me explain the algorithm in plain English:
1. we start at the agent's position in the net (graph)
2. we start breadth-first search of all available connected nodes from the agent's position and add them into the queue, remembering the "start" node of the path (the first node in the agent's path to the gateway)
3. for each element in the queue we repeat the same - add all available connected nodes from the current (head of the queue) queue's node
4. as soon as one of the processing node is a gateway, we know that this is the shortest (one of the shortest) path available for the agent to reach the gateway
5. as we keep the track of the "start" node which leads to this gateway, we output the link "agent position"-"start node position", so we sever the link which leads the shortest path to one of the gateway
6. we update the net (graph) to exclude the severed link
7. and repeat from step 1 based on the new agent's position

Here you can check the replay of one of the test case (the image is clickable):
<a href="https://www.codingame.com/replay/solo/50867723" target="_blank">![Skynet replay](/img/skynet-mission-complete.png)</a>

### Go

```
package main

import (
    "fmt"
    "container/list"
)

// I like the way Go allows you to create new data types, and struct specifically, and the way to construct the struct as well
type Route struct {
    startNode, currentNode int
}

type Link struct {
    from, to int
}

func main() {
    // N: the total number of nodes in the level, including the gateways
    // L: the number of links
    // E: the number of exit gateways
    var N, L, E int
    fmt.Scan(&N, &L, &E)
    
    net := make([][]int, N)
    for i := 0; i < N; i++ {
        // interesting, how you construct multi dimensional slice
        // you declare net as make([][]int, N) saying that net is a slice of N []int (slice) elements
        // and here you initialize every i-th element of the slice (which is a slice as well), but setting the length to zero
        net[i] =  make([]int, 0)
    }
    for i := 0; i < L; i++ {
        // N1: N1 and N2 defines a link between these nodes
        var N1, N2 int
        fmt.Scan(&N1, &N2)
        // here I make every slice element larger if necessary
        net[N1] = append(net[N1], N2)
        net[N2] = append(net[N2], N1)
    }
    gateways := make([]int, 0, 20)
    for i := 0; i < E; i++ {
        // EI: the index of a gateway node
        var EI int
        fmt.Scan(&EI)
        gateways = append(gateways, EI)
    }
    
    for {
        // SI: The index of the node on which the Skynet agent is positioned this turn
        var SI int
        fmt.Scan(&SI)
        // I realized while working on Clojure version of the solution that I could use set instead. Just studied slices at the moment. The "set" can be implemented as a map with value type bool, as stays in Effective Go http://golang.org/doc/effective_go.html#maps
        visited := make([]bool, N)
        visited[SI] = true
        // and use list as a queue implementation
        // list.New() returns a pointer to the list, and all other methods in the package work with list pointer as well
        queue := list.New()
        for i := 0; i < len(net[SI]); i++ {
            startNode := net[SI][i]
            queue.PushBack(Route{startNode: startNode, currentNode: startNode})
        }
        var link *Link
        for e := queue.Front(); e != nil; e = e.Next() {
            route := e.Value.(Route)
            for i := range gateways {
                gateway := gateways[i]
                if route.currentNode == gateway {
                    link = &Link{SI, route.startNode}
                    break
                }
            }
            
            // link must be a pointer to Link struct, otherwise != nil always be true, as the default (empty) value for the struct is the struct with all the fields in it set to its default/empty values, so in our case it would be Link{0, 0}
            // also you can't compare a non-pointer value with a nil, so in case of just a link, you have to &link, but it is never nil
            if link != nil {
                break
            }
            
            visited[route.currentNode] = true
            nodes := net[route.currentNode]
            for i := 0; i < len(nodes); i++ {
                if !visited[nodes[i]] {
                    queue.PushBack(Route{startNode: route.startNode, currentNode: nodes[i]})
                }
            }
        }
        
        fmt.Println(link.from, link.to)
        
        for i, v := range net[link.from] {
            if v == link.to {
                // the trick to remove i-th element from the slice
                // other slice tricks could be found https://github.com/golang/go/wiki/SliceTricks
                net[link.from] = append(net[link.from][:i], net[link.from][i+1:]...)
                break
            }
        }
        
    }
}
```

### Clojure

```
(ns Player
  (:gen-class))

; use defrecord to represent named, well, records aka struct in Go
(defrecord Route [start-node current-node])

(defrecord Link [from to])

; I could have a version of the solution without using atoms, but then the main code would be even more difficult to understand
; {4 [5 8 1], 5 [4], 3 [1 9], 1 [3 4], 8 [4], 9 [3]}
(def net (atom {}))

(def gateways (atom []))

; smaller helper functions are must have in Clojure for better maintanence, testing and readability
(defn- add-link [net from to] 
  (let [nodes (get net from [])] 
    (assoc net from (conj nodes to))))

(defn- unlink [net from to]
  (let [nodes (get net from)]
    (assoc net from (remove #{to} nodes))))

(defn- exclude [from items]
  ; here instead of filter I could use (clojure.set/difference)
  (filter #(not (some #{%} items)) from))

(defn- build-net! [L]
  (loop [i 0]
    (when (< i L) 
      (let [n1 (read) 
            n2 (read)]
        (swap! net add-link n1 n2)
        (swap! net add-link n2 n1)
        (recur (inc i))))))

(defn- read-gateways! [E]
  (loop [i 0]
    (when (< i E)
      (swap! gateways conj (read))
      (recur (inc i)))))

(defn -main [& args]
  (let [N (read) 
        L (read) 
        E (read)]
    (build-net! L)
    (read-gateways! E)
    (loop []
      (let [SI (read)
            nodes-to-routes (fn [nodes] (map #(map->Route {:start-node % :current-node %}) nodes))
            visited #{SI} 
            reachable-agent-next-nodes (get @net SI)
            ; use PersistentQueue/EMPTY to work with queue in Clojure, and use peek, pop and conj to modify/inspect the queue
            routes (apply conj (clojure.lang.PersistentQueue/EMPTY) (nodes-to-routes reachable-agent-next-nodes))]
        (loop [routes routes visited visited]
          (let [route (peek routes) 
                current-node (:current-node route)
                nodes (get @net current-node)
                gateway? (some #{current-node} @gateways)] 
            (if gateway? 
              (let [from SI
                    to (:start-node route)]
                (swap! net unlink from to) 
                (println from to))
              (let [reachable-nodes (exclude (get @net current-node) visited)] 
                (recur
                  (if (seq reachable-nodes)
                    (apply conj (pop routes) (map #(map->Route {:start-node (:start-node route) :current-node %}) reachable-nodes))
                    (pop routes))
                  (conj visited current-node))))))
        (recur)))))

```

### Conclusion

- I found that Clojure program really easy could become unreadable, so it is a best practice there (as in the other languages as well, but if you could skip this step in other languages, which you should not do, in Clojure, if you want to keep your code clean and readable it is a must to do) to extract logic into smaller functions, which are easier to understand and easier to test and work with from REPL, and think carefully about local `let` names


- Writing loops in Clojure feels more work comparing to Go


- Converting solution directly from procedural language to functional language, does not work well, as they have different paradigm and techniques, so it would be faster to write the Clojure version from the beginning rather than converting from Go instruction by instruction


- I like the elegance of Clojure for work with data structures, and using special symbols like ? or ! in the function names


- Both languages feel pleasant and productive to work with, although each of them have it is own specific workflow. I study and practice with Clojure already for quite a long time, while Go was almost new to me, it is really impressive how easy you can go and start coding with Go. And it doesn't feel like a low level language, it is more close to Java, I would say, than to C, but in practice somewhere between, keeping the right balance


- Go and Clojure even share some common ideas as well, channels in Go and `core.async` in Clojure to be precise, although this is not covered in this post


- Not sure if Clojure is the right language for the algorithmic problems, it is for sure a good language of choice for building production systems, but might not be the best one for the algorithmic competitions

And as [Codingame](https://www.codingame.com/) says "Keep Coding!".
