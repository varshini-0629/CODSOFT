# CODSOFT
#!/usr/bin/env python
# coding: utf-8

# In[1]:

#chatbot with rule-based responses
def get_response(user_input: str) -> str:
    msg = user_input.lower().strip()
    if msg == "":
        return "Please say something so I can assist you."
    elif "hello" in msg or "hi" in msg or "hey" in msg or "hii" in msg or "greetings" in msg or "good morning" in msg or "good afternoon" in msg:
        return "Hello! How can I help you today?"
    elif "how are you" in msg:
        return "I'm doing great! How about you?"
    elif "what is your name" in msg or "what's your name" in msg:
        return "I'm ChatBot, your friendly Python helper."
    elif "help" in msg:
        return "I can respond to greetings, questions about me, or say goodbye. What would you like to talk about?"
    elif "bye" in msg or "goodbye" in msg or "see you" in msg:
        return "Goodbye! Have a wonderful day."
    elif "thank you" in msg or "thanks" in msg or "tq" in msg:
        return "You're welcome! Happy to assist."
    elif "how can i download games on mobile?" in msg or "how can i download games on mobile" in msg:
        return "that's a simple process. one can download games and apps through the playstore app present in one's mobile ."
    elif "tell me a joke" in msg or "can you make me laugh?" in msg:
        return "one joke, coming up!what is a sea monster's favourite snack? ships and dip. hope this made you laugh."
    elif "my name is" in msg:
        return "nice to meet you! myself chatbot, your friendly python helper."
    elif "how's the weather outside?":
        return "i recommend you to watch it urself."
    elif msg.endswith("!"):
        return "ohhhh!!"
    else:
        return "Sorry, I didn't quite get that. Can you rephrase?"

def chat():
    print("Hi! I'm a simple Python chatbox. Type 'exit' or 'quit' to leave.")
    while True:
        user_input = input("You: ").strip()
        if user_input.lower() in ('exit', 'quit'):
            print("ChatBot: Goodbye! Exiting chat.")
            break
        response = get_response(user_input)
        print("ChatBot:", response)

if __name__ == "__main__":
    chat()


# In[2]:

#tic-tac-toe AI
import math

def print_board(board):
    print()
    print(f" {board[0]} | {board[1]} | {board[2]} ")
    print("---+---+---")
    print(f" {board[3]} | {board[4]} | {board[5]} ")
    print("---+---+---")
    print(f" {board[6]} | {board[7]} | {board[8]} ")
    print()

def check_winner(board, player):
    # Winning combinations
    win_combinations = [
        (0,1,2),(3,4,5),(6,7,8), # rows
        (0,3,6),(1,4,7),(2,5,8), # columns
        (0,4,8),(2,4,6)          # diagonals
    ]
    for combo in win_combinations:
        if all(board[i] == player for i in combo):
            return True
    return False

def is_board_full(board):
    return all(space != ' ' for space in board)

def get_available_moves(board):
    return [i for i, spot in enumerate(board) if spot == ' ']

def minimax(board, depth, is_maximizing, alpha, beta):
    # AI is 'O', Human is 'X'
    if check_winner(board, 'O'):
        return 10 - depth
    if check_winner(board, 'X'):
        return depth - 10
    if is_board_full(board):
        return 0

    if is_maximizing:
        max_eval = -math.inf
        for move in get_available_moves(board):
            board[move] = 'O'
            eval = minimax(board, depth+1, False, alpha, beta)
            board[move] = ' '
            max_eval = max(max_eval, eval)
            alpha = max(alpha, eval)
            if beta <= alpha:
                break
        return max_eval
    else:
        min_eval = math.inf
        for move in get_available_moves(board):
            board[move] = 'X'
            eval = minimax(board, depth+1, True, alpha, beta)
            board[move] = ' '
            min_eval = min(min_eval, eval)
            beta = min(beta, eval)
            if beta <= alpha:
                break
        return min_eval

def get_best_move(board):
    best_score = -math.inf
    best_move = None
    for move in get_available_moves(board):
        board[move] = 'O'
        score = minimax(board, 0, False, -math.inf, math.inf)
        board[move] = ' '
        if score > best_score:
            best_score = score
            best_move = move
    return best_move

def player_move(board):
    while True:
        try:
            user_input = input("Your move (1-9): ")
            pos = int(user_input) - 1
            if pos < 0 or pos > 8:
                print("Please enter a number from 1 to 9.")
            elif board[pos] != ' ':
                print("That position is already taken. Try again.")
            else:
                board[pos] = 'X'
                break
        except ValueError:
            print("Invalid input. Enter a number from 1 to 9.")

def main():
    print("Welcome to Tic-Tac-Toe!")
    print("You are X, AI is O.")
    print("Positions are numbered 1 to 9 like this:")
    print(" 1 | 2 | 3 ")
    print("---+---+---")
    print(" 4 | 5 | 6 ")
    print("---+---+---")
    print(" 7 | 8 | 9 ")
    board = [' '] * 9

    while True:
        print_board(board)
        player_move(board)

        if check_winner(board, 'X'):
            print_board(board)
            print("Congratulations! You win!")
            break

        if is_board_full(board):
            print_board(board)
            print("It's a draw!")
            break

        print("AI is making a move...")
        ai_move = get_best_move(board)
        board[ai_move] = 'O'

        if check_winner(board, 'O'):
            print_board(board)
            print("AI wins! Better luck next time.")
            break

        if is_board_full(board):
            print_board(board)
            print("It's a draw!")
            break

if __name__ == "__main__":
    main()


# In[3]:

#recommendation system
import math

# Example movie dataset with genres represented in keywords
movies = {
    'The Matrix': ['sci-fi', 'action', 'dystopian'],
    'The Godfather': ['crime', 'drama', 'classic'],
    'Inception': ['sci-fi', 'action', 'thriller'],
    'Pride and Prejudice': ['romance', 'classic', 'drama'],
    'The Terminator': ['action', 'sci-fi', 'thriller'],
    'The Notebook': ['romance', 'drama'],
    'The Shawshank Redemption': ['drama', 'crime', 'classic'],
    'The Avengers': ['action', 'sci-fi', 'superhero'],
    'Titanic': ['romance', 'drama', 'classic'],
    'The Dark Knight': ['action', 'crime', 'superhero']
}

def build_genre_vectors(movies):
    # Build a set of all genres
    all_genres = set()
    for genres in movies.values():
        all_genres.update(genres)
    all_genres = sorted(all_genres)
    
    # Create a mapping from genre to index
    genre_index = {genre: i for i, genre in enumerate(all_genres)}
    
    # Build vector representation for each movie
    movie_vectors = {}
    for movie, genres in movies.items():
        vector = [0] * len(all_genres)
        for genre in genres:
            vector[genre_index[genre]] = 1
        movie_vectors[movie] = vector
    return movie_vectors, genre_index

def cosine_similarity(vec1, vec2):
    dot = sum(a*b for a,b in zip(vec1, vec2))
    norm1 = math.sqrt(sum(a*a for a in vec1))
    norm2 = math.sqrt(sum(b*b for b in vec2))
    if norm1 == 0 or norm2 == 0:
        return 0.0
    return dot / (norm1 * norm2)

def get_user_profile(preferences, genre_index):
    # Build user profile vector based on liked movie genres
    user_vector = [0] * len(genre_index)
    for movie in preferences:
        if movie in movies:
            genres = movies[movie]
            for genre in genres:
                idx = genre_index[genre]
                user_vector[idx] += 1
        else:
            print(f"Warning: '{movie}' not found in movie list.")
    # Normalize user vector
    total = sum(user_vector)
    if total > 0:
        user_vector = [x/total for x in user_vector]
    return user_vector

def recommend_movies(user_profile, movie_vectors, preferences, top_n=5):
    scores = {}
    for movie, vector in movie_vectors.items():
        if movie in preferences:
            continue  # skip movies the user already likes
        sim = cosine_similarity(user_profile, vector)
        scores[movie] = sim
    # Sort movies by similarity descending
    ranked = sorted(scores.items(), key=lambda x: x[1], reverse=True)
    return ranked[:top_n]

def main():
    print("Welcome to the Simple Movie Recommender!")
    print("Here's the list of movies you can choose from as your preferences:")
    for movie in movies:
        print("-", movie)

    user_input = input("\nEnter the movies you like from the list above, separated by commas:\n")
    preferences = [m.strip() for m in user_input.split(',') if m.strip()]

    movie_vectors, genre_index = build_genre_vectors(movies)
    user_profile = get_user_profile(preferences, genre_index)
    recommendations = recommend_movies(user_profile, movie_vectors, preferences, top_n=5)

    print("\nBased on your preferences, we recommend these movies:")
    for movie, score in recommendations:
        print(f"{movie} (Similarity: {score:.2f})")

if __name__ == "__main__":
    main()