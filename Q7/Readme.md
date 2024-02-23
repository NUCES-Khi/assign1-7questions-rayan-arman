# Assignment 1 : Question 7
|Std ID|Name|
|------|-|
|K228708|Arman Faisal|
|K224102|Rayan Farhan|

#no solutions found using hill climb that's why i've done using backtracking too

import random

def is_valid(queens, row, col):
  #Check if any queen in the same row or column is attacking
  for i in range(len(queens)):
    if queens[i] == col or (abs(i - row) == abs(queens[i] - col)):
      return False
  return True

def get_attacking_pairs(queens):
  # Count the number of attacking pairs of queens
  attacks = 0
  for i in range(len(queens)):
    for j in range(i + 1, len(queens)):
      if queens[i] == queens[j] or (abs(i - j) == abs(queens[i] - queens[j])):
        attacks += 1
  return attacks

def get_random_neighbor(queens):
  #Swapping two queens randomly
  i, j = random.sample(range(len(queens)), 2)
  queens[i], queens[j] = queens[j], queens[i]
  return queens

def hill_climbing(n):
  #Initializeboard
  queens = [random.randint(0, n - 1) for _ in range(n)]

  while True:
    #Get attacking pairs current
    current_attacks = get_attacking_pairs(queens)

    #Check if all queens are safe
    if current_attacks == 0:
      return queens

    #Exploring neighbors
    improved = False
    for i in range(n):
      for j in range(n):
        if i != j:
          neighbor = queens[:]
          neighbor[i] = j
          if is_valid(neighbor, i, j):
            neighbor_attacks = get_attacking_pairs(neighbor)
            if neighbor_attacks < current_attacks:
              queens = neighbor
              improved = True
              break
      if improved:
        break

    if not improved:
      return None

n = 8
solution = hill_climbing(n)

if solution:
  print("Solution found:")
  for i in range(n):
    print(f"[{' '.join(['-' for _ in range(solution[i])] + ['Q'] + ['-' for _ in range(n - solution[i] - 1)])}]")
else:
  print("No solution found.")



#Using backtracking

def solve_n_queens(n):
    def is_safe(board, row, col):
        for i in range(col):
            if board[row][i] == 1:
                return False
        for i, j in zip(range(row, -1, -1), range(col, -1, -1)):
            if board[i][j] == 1:
                return False
        for i, j in zip(range(row, n, 1), range(col, -1, -1)):
            if board[i][j] == 1:
                return False

        return True

    def solve_n_queens_rec(board, col):
        if col >= n:
            return True
        for i in range(n):
            if is_safe(board, i, col):
                board[i][col] = 1
                if solve_n_queens_rec(board, col + 1) == True:
                    return True
                board[i][col] = 0 #backtrack
        return False

    board = [[0]*n for _ in range(n)]

    if solve_n_queens_rec(board, 0) == False:
        print("Solution does not exist")
        return False
    for i in range(n):
        for j in range(n):
            print(board[i][j], end = " ")
        print()

n = int(input("Enter the number of queens: "))
solve_n_queens(n)
