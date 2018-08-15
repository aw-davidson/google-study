##Maximum Product of Word Lengths
Given a string array words, find the maximum value of length(word[i]) * length(word[j]) where the two words do not share common letters. You may assume that each word will contain only lower case letters. If no such two words exist, return 0.

Example 1:

Input: ["abcw","baz","foo","bar","xtfn","abcdef"]
Output: 16
Explanation: The two words can be "abcw", "xtfn".

```javascript
var maxProduct = function(words) {
    let bitArr = [];
    let max = 0;
    for (let word of words) {
        let bitWord = 0;
        for (let char of word) {
            bitWord |= 1 << (char.charCodeAt(0) - 'a'.charCodeAt(0))
        }
        bitArr.push(bitWord);
    }

    for (let i = 0; i < words.length; i++) {
            for (let j = 1; j < words.length; j++) {
                if((bitArr[i] & bitArr[j]) == 0) {
                    max = Math.max(max, words[i].length * words[j].length);
                }
            }
        }

    return max;

};
```

'abc' gets turned into '111' and 'd' gets turned into '1000'. The numbers are or'd to determine if they have any common charachters.

##canFinish
There are a total of n courses you have to take, labeled from 0 to n-1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?

Example 1:

Input: 2, [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take.
             To take course 1 you should have finished course 0. So it is possible.
Example 2:

Input: 2, [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take.
             To take course 1 you should have finished course 0, and to take course 0 you shou
```javascript
var canFinish = function(numCourses, prerequisites) {
    if (!numCourses || numCourses === 0 || !prerequisites || prerequisites.length === 0) { return true; }

    let inDegree = {};
    let edges = {};
    for (let i = 0; i < numCourses; i++) {
        inDegree[i] = 0;
        edges[i] = [];
    }

    for (let i = 0; i < prerequisites.length; i++) {
        let preq = prerequisites[i];
        inDegree[preq[0]]++;
        edges[preq[1]].push(preq[0]);
    }

    let queue = [];
    for (let i = 0; i < numCourses; i++) {
        if (inDegree[i] === 0) {
            queue.push(i);
        }
    }

    if (queue.length === 0) { return false; }

    while (queue.length > 0) {
        let node = queue.shift();
        for (let i = 0; i < edges[node].length; i++) {
            let nextNode = edges[node][i];
            inDegree[nextNode]--;
            if (inDegree[nextNode] === 0) {
                queue.push(nextNode);
            }
        }
    }

    let count = 0;
    Object.keys(inDegree).forEach(key => {
        if (inDegree[key] === 0) { count++; }
    });

    return count === numCourses;

};


```

We create an adjency list of the edges and and keep track of the indegrees.


```javascript
var moveUp = function(robot){
    return robot.move()
}

var moveLeft = function(robot){
    robot.turnLeft()
    var ok = robot.move()
    robot.turnRight()
    return ok
}

var moveRight = function(robot){
    robot.turnRight()
    var ok = robot.move()
    robot.turnLeft()
    return ok
}

var moveDown = function(robot){
    robot.turnRight()
    robot.turnRight()
    var ok = robot.move()
    robot.turnLeft()
    robot.turnLeft()
    return ok
}

var cleanCell = function(robot, visited, x, y){
    //we keep track of cells that we have already visited and return if we have already visited this cell
    var id = x+","+y;
    if(visited[id]){
        return;
    }
    //else we clean the room and add it visited
    robot.clean()
    visited[id] = 1

    //if the robot can move up
    if(moveUp(robot)){
        //clean the cell and move back down to continue backtracking
        cleanCell(robot, visited, x,y-1)
        moveDown(robot)
    }
    if(moveLeft(robot)){
        cleanCell(robot, visited, x-1,y)
        moveRight(robot)
    }
    if(moveRight(robot)){
        cleanCell(robot, visited, x+1,y)
        moveLeft(robot)
    }
    if(moveDown(robot)){
        cleanCell(robot, visited, x,y+1)
        moveUp(robot)
    }

}

var cleanRoom = function(robot) {
    visited = {}
    cleanCell(robot, visited, 0, 0)
};
```

##Closest Binary Search Tree Value
Given a non-empty binary search tree and a target value, find the value in the BST that is closest to the target.

Note:

Given target value is a floating point.
You are guaranteed to have only one unique value in the BST that is closest to the target.
Example:

Input: root = [4,2,5,1,3], target = 3.714286

    4
   / \
  2   5
 / \
1   3

Output: 4

```javascript
var closestValue = function(root, target) {
    let a = root.val;
    let kid = target < a ? root.left : root.right;
    if (kid == null) return a;
    let b = closestValue(kid, target);
    return Math.abs(a - target) < Math.abs(b - target) ? a : b;
};
```

When we reach 3 we return its value to 2 becuase the rightmost node is null. We return 3 to 4 becuase 3 is closer to the target than 2. Then we return 4 which is closer to the target than 3.

##Course Schedule
There are a total of n courses you have to take, labeled from 0 to n-1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?

Example 1:

Input: 2, [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take.
             To take course 1 you should have finished course 0. So it is possible.
Example 2:

Input: 2, [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take.
             To take course 1 you should have finished course 0, and to take course 0 you should
             also have finished course 1. So it is impossible.
Note:

The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about how a graph is represented.
You may assume that there are no duplicate edges in the input prerequisites.

```javascript

```
for each unvisited edge (u, v) in Set E
  if (find(u) === find(v)) {
    //cycle detected
  } else {
    union(x, y) //union of sets that they belong to
  }



##validate BST
```javascript
var isValidBST = function(root, upper = Infinity, lower = -Infinity) {
    if (root === null) return true;
    if (root.val <= lower || root.val >= upper) return false;
    return isValidBST(root.left, root.val, lower) && isValidBST(root.right, upper, root.val);
};

var isValidBST = function(root) {

    let prev = null;
    let currentNode = root;
    let stack = [];

    while (stack.length || currentNode != null) {
        if (currentNode) {
            stack.push(currentNode);
            currentNode = currentNode.left;
        } else {
            let p = stack.pop() ;
		    if (prev != null && p.val <= prev.val) {
			    return false ;
		    }
		    prev = p;
			currentNode = p.right;

        }
    }

    return true;


};
```

inorder succesor

```javascript
var inorderSuccessor = function(root, p) {

    if (root == null) return null;

    if (root.val <= p.val) {
        return inorderSuccessor(root.right, p);
    } else {
        let left = inorderSuccessor(root.left, p);
        return left ? left : root;
  }
};
```

## One Edit Distance

Given two strings s and t, determine if they are both one edit distance apart.

Note:

There are 3 possiblities to satisify one edit distance apart:

Insert a character into s to get t
Delete a character from s to get t
Replace a character of s to get t
Example 1:

Input: s = "ab", t = "acb"
Output: true
Explanation: We can insert 'c' into s to get t.
Example 2:

Input: s = "cab", t = "ad"
Output: false
Explanation: We cannot get t from s by only one step.
Example 3:

Input: s = "1203", t = "1213"
Output: true
Explanation: We can replace '0' with '1' to get t.
```javascript
var isOneEditDistance = function(s, t) {
    if (s === t) return false;

    //insert and delete
    if (Math.abs(s.length - t.length) === 1) {
        let j;
        let shorter = s.length < t.length ? s : t;
        let longer = s.length >= t.length ? s : t;
        console.log(shorter, longer)
        for (let i = 0, j = 0; i < shorter.length; i++, j++) {
            let matched = true;
            if (matched && shorter[i] !== longer[j]) {
                j++;
                matched = false;
                if (shorter[i] !== longer[j]) return false;
            } else if (shorter[i] !== longer[j]) {
                return false
            }
        }

        return true;
    }

    //replace
    if (s.length == t.length) {
        let count = 0;
        for (let i = 0; i < s.length; i++) {
            if (s[i] !== t[i]) count++;
            if (count > 1) return false
        }

        return true;
    }

    return false;


};
```
##Design Tic-Tac-Toe


```javascript
var TicTacToe = function(n) {
    this.rows = new Array(n);
    this.cols = new Array(n);
    this.diagonal = 0;
    this.antiDiagonal = 0;

    this.rows.fill(0);
    this.cols.fill(0);


};

TicTacToe.prototype.move = function(row, col, player) {
    let val = player === 1 ? 1 : -1;
    let n = this.rows.length;

    this.rows[row] += val;
    this.cols[col] += val;

    if (row === col) {
        this.diagonal += val;
    }

    if (row + col === n - 1) {
        this.antiDiagonal += val;
    }

    if (
        Math.abs(this.rows[row]) === n ||
        Math.abs(this.cols[col]) === n ||
        Math.abs(this.diagonal) === n ||
        Math.abs(this.antiDiagonal) === n
       ) {
        return player;
    }

    return 0;

};
```

The naive approach to checking for a win is to keep track of the entire board and check for horizontal, vertical and diagonals. The trick to this problem is to think about the condition for winning. Winning means that a player has n marks along a row, col, or diagonal. Therefore we can check for a win by keeping track of an array of n that represents the state of the cols. one element represents one column. In this case we choose for player 1 to be 1 and player 2 to be -1. if the abs value of any of cols row and diags is equal to n then there is a win. The diagonal move occurs when row = col or when row + col = n - 1.

##same tree
Given two binary trees, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical and the nodes have the same value.
```javascript
var isSameTree = function(p, q) {

	if(p===null && q==null) return true;
	if(p===null || q===null) return false;

	return p.val===q.val && isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
};
```

Linear time. The recursive solution has O(h) memory complexity as it will consume memory on the stack up to the height of binary tree h. It will be O(log n) for a balanced tree and in the worst case can be O(n).








###General Study


Trie

```javascript
function TrieNode() {
    this.children = new Map();
    this.end = false;
}
var Trie = function() {
    this.root = new TrieNode();
};

/**
 * Inserts a word into the trie.
 * @param {string} word
 * @return {void}
 */
Trie.prototype.insert = function(word, node = this.root) {

    if (!word.length) {
        node.end = true;
        return;
    } else if (!node.children.has(word[0])) {
        node.children.set(word[0], new TrieNode())
    }

   return this.insert(word.substr(1), node.children.get(word[0]));

};

/**
 * Returns if the word is in the trie.
 * @param {string} word
 * @return {boolean}
 */
Trie.prototype.search = function(word, node = this.root) {

    if (node.children.has(word[0])) {

        if (node.children.get(word[0]).end && word.length == 1) return true;

        return this.search(word.substr(1), node.children.get(word[0]))
    }

    return false;
};

/**
 * Returns if there is any word in the trie that starts with the given prefix.
 * @param {string} prefix
 * @return {boolean}
 */
Trie.prototype.startsWith = function(prefix, node = this.root) {

    if (!prefix.length) return true;


    if (node.children.has(prefix[0])) {


        return this.startsWith(prefix.substr(1), node.children.get(prefix[0]))
    }

    return false;
};
```
##Union find for detecting cycle in undirected graph



```javascript
function hasCycle(edges) {
    //initialize parent so that parent[node.val] = -1 and is self-representing
    let parent = [];
    for (let i = 1; i <= edges.length; i++) {
        parent[i] = -1;
    }
    //iterate through all edges
    for (let i = 0; i < edges.length; i++) {
        let u = edges[i][0];
        let v = edges[i][1];

        if (find(parent, u) === find(parent, v)) {
            //cycle detected
            console.log(u, v)
            return true;
        } else {
            union(u, v, parent)
        }
    }

    return false;
}

function find(parent, i) {
    if (parent[i] === -1) {
        return i;
    }
    //if parent[i] does not equal -1 the node is not self-reoresenting, so find the self-representing         node which represents the set
    return find(parent, parent[i]);
}

function union(x, y, parent) {
    let xset = find(parent, x);
    let yset = find(parent, y);
    parent[xset] = yset;
}
```

##How do you make a function that only calls input function f every 50 milliseconds?

```javascript
function throttle(func, delay) {
  let wait = false;
  return function(){
    if (!wait) {
        func.call();
        wait = true;
        setTimeout(() => {
        wait = false;
      }, delay)
    }
  }
}
```

##How do you make a function that takes f and returns a function that calls f on a timeout?

```javascript
// Returns a function, that, as long as it continues to be invoked, will not
// be triggered. The function will be called after it stops being called for
// N milliseconds.

function debounce(func, delay) {
  let timeoutID;
  return function () {
    let context = this;
    let args = arguments;
    clearTimeout(timeoutID);
    timeoutID = setTimeout(() => {
      func.apply(context, args);
    }, delay)
  }
}
```

##Check if matrix is word square (check [i][j] === [j][i])

##Minesweeper problem. Write a function reveal() that outputs the number of tiles shown when a user clicks on a tile. Each tile shows the number of bombs as its neighbor. If the user click on a tile that is a bomb, the game is over. If that tile is 0, reveal all its neighbors.

##design algorithm for tic-tac-toe

##Design a webpage which can auto post new posts when you reach the bottom of the page by using javascript. So you may use AJAX and some javascript event listeners. But, they do not require you remember the functions names, it will be great as long as you can describe your thought and design.

```javascript

function autoPost(event) {
  //scrollPos is top of window. 0 at the start and some number at the bottom of the page.
  //if scrollHeight + pageHeight = windowHeight then we are at the bottom
  if (element.scrollHeight - element.scrollTop === element.clientHeight)
    {
      let element = event.target;
      let url = 'http://google.com'
      let form = new FormData(document.getElementById('login-form'));
       return fetch(url, {
          method: 'POST',
          body: form
        })
        .then((res) => res.data)
        .catch(err => console.error('Fetch error' , err))
    }
}

function throttle(func, delay) {

}

window.addEventListener('scroll', throttle(autoPost))

```

##Describe what happens when a user clicks a link in a browser.
A link will have an href attribute which specefies the URL of the next webpage that you want to view. The browser takes you to that URL by using the DNS (Domain Name System) to get an IP for that site. Your browser then opens a tcp connection to the website over IP. The browser asks to speak to the http server which then provides the next web page in HTML that your browser renders.

##Throttle, debounce, setInterval

```javascript
function throttle(func, delay) {
  let wait = false;
  return function() {
    if (!wait) {
      func(...Array.from(arguments));
      wait = true;
      setTimeout(() => {
        wait = false;
      }, delay)
    }
  }
}

function debounce(func, delay) {
  let timouetId;
  return function() {
    let context = this;
    let args = arguments;
    if (timeoutId) {
      clearTimeout(timeoutId);
    }
    timouetId = setTimeout(() => {
      func.apply(context, args);
    }, delay)
  }
}

function callInterval(func) {
  setInterval(func.bind(null, ...Array.from(arguments).slice(1)), 500)
}

callInterval(console.log, 'hello', 'bye')

```
