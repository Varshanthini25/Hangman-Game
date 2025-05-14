import random
import time

# Dictionary of categories with words
categories = {
    "fruits": ["apple", "banana", "mango", "grape", "orange", "pineapple", "strawberry","blueberry","guva","kiwi"],
    "animals": ["elephant", "tiger", "giraffe", "kangaroo", "panda", "zebra", "lion","horse","rabbit","pig"],
    "birds": ["parrot", "eagle", "penguin", "sparrow", "peacock", "owl", "flamingo","crow","dove"],
    "sports": ["football", "cricket", "tennis", "badminton", "basketball", "hockey", "volleyball","golf","boxing"],
    "planets": ["mercury", "venus", "earth", "mars", "jupiter", "saturn", "uranus", "neptune"],
    "trees": ["oak", "pine", "maple", "willow", "mango", "coconut", "baobab","apple","neem"],
    "technologies": ["computer", "internet", "software", "hardware", "robotics", "artificial", "blockchain","cloud computing","edge computing"]
}

def get_new_word(category):
    """Selects a new random word from the chosen category."""
    return random.choice(categories[category])

def hangman_game():
    print("\n🎮 Welcome to Hangman!")
    print("Choose a category:")
    for category in categories.keys():
        print("-", category.capitalize())

    # Get category choice from user
    while True:
        category = input("\nEnter a category: ").lower()
        if category in categories:
            break
        print("❌ Invalid category! Please choose from the list.")

    word = get_new_word(category)
    word_display = ["_"] * len(word)  # Display underscores for unguessed letters
    attempts = 7 # Maximum incorrect guesses allowed
    guessed_letters = set()
    start_time = time.time()  # Track time for auto-reveal

    print("\n📌 Category:", category.capitalize())
    print("Guess the word, one letter at a time.")
    print("Type 'skip' if the word is too hard.")
    print("You have", attempts, "incorrect guesses allowed.")
    print("Word:", " ".join(word_display))

    while attempts > 0 and "_" in word_display:
        # Check if time exceeded (30 seconds)
        if time.time() - start_time > 60:
            print("\n⏳ Time's up! The word was:", word)
            word = get_new_word(category)
            word_display = ["_"] * len(word)
            attempts = 7
            guessed_letters.clear()
            start_time = time.time()
            print("\n🔄 New word selected! Try again.")
            print("Word:", " ".join(word_display))
            continue

        guess = input("\nEnter a letter (or type 'skip' to change the word): ").lower()

        # Allow skipping the word
        if guess == "skip":
            word = get_new_word(category)
            word_display = ["_"] * len(word)
            attempts = 7
            guessed_letters.clear()
            start_time = time.time()
            print("\n🔄 New word selected! Try again.")
            print("Word:", " ".join(word_display))
            continue

        # Validate input
        if len(guess) != 1 or not guess.isalpha():
            print("❌ Invalid input! Please enter a single letter.")
            continue

        # Check if letter was already guessed
        if guess in guessed_letters:
            print("⚠️ You already guessed that letter!")
            continue

        guessed_letters.add(guess)

        # Check if guess is correct
        if guess in word:
            print("✅ Good job! The letter is in the word. 😊")
            for i in range(len(word)):
                if word[i] == guess:
                    word_display[i] = guess
        else:
            attempts -= 1
            print("❌ Wrong guess! You have", attempts, "attempts left. 😢")

        print("Word:", " ".join(word_display))

    # Game result
    if "_" not in word_display:
        print("\n🎉 Congratulations! You guessed the word:", word)
    else:
        print("\n💀 Game Over! The correct word was:", word)

# Run the game
hangman_game()
