import math

def alphabeta_graph(graph, current_node, depth, is_maximizer, alpha, beta):
    if depth == 0 or 'value' in graph[current_node]:
        return graph[current_node].get('value', 0), [current_node]
    
    if is_maximizer:
        best_value = -math.inf
        best_path = []
        
        for neighbor in graph[current_node].get('children', []):
            value, path = alphabeta_graph(graph, neighbor, depth - 1, False, alpha, beta)
            
            if value > best_value:
                best_value = value
                best_path = [current_node] + path
            
            alpha = max(alpha, best_value)
            if beta <= alpha:
                print(f"  Pruned at {current_node}: remaining children {graph[current_node]['children'][graph[current_node]['children'].index(neighbor)+1:]}")
                break
                
        return best_value, best_path
    
    else:
        best_value = math.inf
        best_path = []
        
        for neighbor in graph[current_node].get('children', []):
            value, path = alphabeta_graph(graph, neighbor, depth - 1, True, alpha, beta)
            
            if value < best_value:
                best_value = value
                best_path = [current_node] + path
            
            beta = min(beta, best_value)
            if beta <= alpha:
                print(f"  Pruned at {current_node}: remaining children {graph[current_node]['children'][graph[current_node]['children'].index(neighbor)+1:]}")
                break
                
        return best_value, best_path

game_tree = {
    'A': {'children': ['B', 'C']},
    'B': {'children': ['D', 'E']},
    'C': {'children': ['F', 'G']},
    'D': {'children': ['H', 'I']},
    'E': {'children': ['J', 'K']},
    'F': {'children': ['L', 'M']},
    'G': {'children': ['N', 'O']},
    'H': {'value': -1},
    'I': {'value': 4},
    'J': {'value': 2},
    'K': {'value': 6},
    'L': {'value': -3},
    'M': {'value': -5},
    'N': {'value': 0},
    'O': {'value': 7}
}

print("Alpha-Beta Pruning Trace:")
print("=" * 40)
value, path = alphabeta_graph(game_tree, 'A', 3, True, -math.inf, math.inf)
print("=" * 40)
print(f"\nScore: {value}")
print(f"Optimal Path: {' → '.join(path)}")

