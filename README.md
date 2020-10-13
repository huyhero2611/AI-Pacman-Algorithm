# AI-Pacman-Algorithm
Các bài toán đã được giải quyết trên Project:

1. DFS
* Tạo Stack để lưu các state và actions nó đi, thêm vị trí bắt đầu vào để bắt đầu chạy. Thêm 1 list Visited để kiểm tra các node đã đi qua.
    ```
    start_state = problem.getStartState() 
    stack = util.Stack() 
    stack.push( (start_state, []) )  
    visited = []
    ```
* Kiểm tra 
  - Nếu state hiện tại là goal thì trả về actions
    ```
    if problem.isGoalState(current_state):
            return actions
    ```            
  - Nếu không phải thì tạo 1 successors để add state và action vào Stack
    ```
    successors = problem.getSuccessors(current_state)
    for state, action, cost in successors:
        if state not in visited:
            stack.push((state, actions+[action]))
    ```            
2. BFS
* Tạo Queue để lưu các state và actions nó đi, thêm vị trí bắt đầu vào để bắt đầu chạy. Thêm 1 list Visited để kiểm tra các node đã đi qua.
    ```
    start_state = problem.getStartState() 
    queue = util.Queue() 
    queue.push((start_state, []))
    visited = [] 
    ```
* Kiểm tra 
  - Nếu state hiện tại là goal thì trả về actions
    ```
    if problem.isGoalState(current_state):
            return actions
    ```
  - Nếu không phải thì tạo 1 successors để add state và action vào Queue
    ```
    successors = problem.getSuccessors(current_state)
    for state, action, cost in successors:
        if state not in visited:
            queue.push((state, actions+[action]))
    ```
3. UCS
* Tạo Priority Queue để lưu các state và actions nó đi, thêm vị trí bắt đầu vào để bắt đầu chạy. Thêm 1 list Visited để kiểm tra các node đã đi qua.
    ```
    start_state = problem.getStartState()
    pri_queue = util.PriorityQueue()
    pri_queue.push((start_state, []), 0) 
    visited = []
    ```
* Kiểm tra 
  - Nếu state hiện tại là goal thì trả về actions
    ```
    if problem.isGoalState(current_state):
            return actions
    ```                       
  - Nếu không phải thì tạo 1 successors để add state và action vào Priority Queue. Tại đây sẽ có thêm getCostActions để tính toán   chi phí đường đi với mục đich sẽ pop đường đi có chi phí ít nhất
    ```
    successors = problem.getSuccessors(current_state)
    for state, action, cost in successors:
        if state not in visited:
            pri_queue.push((state, actions+[action]), problem.getCostOfActions(actions + [action]))
    ```            
4. A*
* Tạo Priority Queue để lưu các state và actions nó đi, thêm vị trí bắt đầu vào để bắt đầu chạy. Thêm 1 list Visited để kiểm tra các node đã đi qua.
    ```
    start_state = problem.getStartState()
    pri_queue = util.PriorityQueue()
    pri_queue.push((start_state, []), 0) 
    visited = []
    ```
* Kiểm tra 
  - Nếu state hiện tại là goal thì trả về actions
    ```
    if problem.isGoalState(current_state):
            return actions
    ```            
  - Nếu không phải thì tạo 1 successors để add state và action vào Priority Queue. Tại đây sẽ có thêm getCostActions để tính toán   chi phí đường đi + 1 hàm dự đoán với mục đich tối ưu chi phí khi pop ra để kiểm tra
    ```
    successors = problem.getSuccessors(current_state)
    for state, action, cost in successors:
        if state not in visited:
            pri_queue.push((state, actions+[action]), problem.getCostOfActions(actions + [action]))
    ```
5. Finding All the Corners
a. Init
- Tại đây sẽ tạo 1 list unvisited corners để kiểm tra nếu pacman đã đi qua góc chưa
b. getStartState
- trả về 1 mảng gồm 2 giá trị
  + vị trí bắt đâu StartingPosition
  + list check unvisitedCorner
c. isGoalState 
- trả về đúng nếu không còn state corner nào trong state[1]
d. successor
- lấy các thành phần cần thiết để xử lí 
  ```
  x, y = state[0]
  dx, dy = Actions.directionToVector(action)
  nextx, nexty = int(x + dx), int(y + dy)
  hitsWall = self.walls[nextx][nexty]
  ```
- xét nếu mà không phải tường sẽ add vị trí tiếp theo vào list successors và remove các góc đã đi qua trong state[1] nếu vị trí tiếp theo là corner
  if not hitsWall:
  ```
  nextState = [(nextx, nexty), state[1][:]]
  if nextState[0] in self.corners and nextState[0] in nextState[1]:
      nextState[1].remove(nextState[0])
  successor = (nextState, action, 1)
  successors.append(successor)
  ```
6. Heuristic
- tạo 1 hàm manhattan để tính toán vị trí hiện tại tới vị trí mong muốn
  ```
  def manhattan(x1, y1, x2, y2):
    return abs(x1 - x2) + abs(y1 - y2)
  ```
- for các corner để kiểm tra nếu corner có trong state[1] thì ta sẽ tính khoảng cách đến corner đó. Nếu khoảng cách đó > h* thì gán h* = distances
  for corner in corners:
  ```
  if corner in state[1]:
      distances = manhattan(corner[0], corner[1], currentPosition[0], currentPosition[1])
      if distances > hStar:
          hStar = distances
  ```
7. Eat All Dots
- lấy ra danh sách các foods xuất hiện trên ma trận
```
foodPosition = foodGrid.asList()
```
- duyệt toàn bộ foods và xét heuristic* = max heuristic tính được trong foods position
```
heuristic = [0] 
for pos in foodPosition:
    heuristic.append(mazeDistance(position,pos,problem.startingGameState)) # add cac heuristic cua food vao mang heuristic
return max(heuristic)
```
8. suboptimal search
- xét bất cứ food position nào đều là goal => eat all dots (in AnyFoodSearchProblem)
```
return state in self.food.asList()
```
- sau đó truyền vào ClosestDotSearchAgent và sử dụng BFS để giải quyết bài toán
```
from search import breadthFirstSearch # worse case = depth first search
return breadthFirstSearch(problem)
```
 
