# Scrambler-Game
Fun game unscrambling words
Copy rows 6 to 76


# steps to interact with player
# 3. until correct or want to quit:
# 3a. unscramble this word: PEPLA
# 4. possible responses: APPLE, PAPEL/ALPEP, quit!, PAPER, hint?
# steps to evaluate their response
# 5.   get response
# 6.    if response is correct, 'you got it!' / game over
# 7.    elif response is PAPEL, 'incorrect'
# 8.    elif response is QUIT, then quit / stop asking them
# 9.    elif response is HINT, then tell them the next letter
#            need some way to keep track of either
#                 - how many hints so far
#                 - position of next letter to be revealed
#            'the first letter is "A"'
#            'the next letter is "P"'
import random # make the Python random module available to us to use
words = open('wordlist.txt').read().upper().split()
word = random.choice(words) # pick a word at random and make it upper case
letters = list(word) # 2
# we need to keep track of the which hint indexes we have already given to the user
# first hint is always first letter (index 0), so not included here
# second hint is always last letter (index len()-1), so not included here
hint_indices = list(range(1, len(word) - 1)) 
random.shuffle(letters) # 2
number_of_hints = 0 # number of hints given so far
guesses = 1

while True: # while they haven't guess it or haven't asked to quit
    print("Unscramble these letters into a word:", ''.join(letters)) 
    print("Type 'q' to quit, or 'h' for a hint.")
    response = input('? ').upper() # 5 
    
    if response == word: # 6
        print('You got it in', guesses, 'guesses and', number_of_hints, 'hints!')
        break
    if response == 'Q': # quit
        print('Thanks for playing!')
        print('The word was', word)
        break
    if response == 'H': # hint
        # first hint is always first letter
        # second hint is always last letter
        # subsequent hints are a random letter (that we haven't already given them)
        if number_of_hints == 0: # no hints so far, therefore this is first hint
            print('the first letter is', word[0])
            number_of_hints += 1
        elif number_of_hints == 1: # second hint
            print('the last letter is', word[-1])
            number_of_hints += 1  
        # if neither of the above them we give a random hint
        # RAILROAD ... -AILROA- ... -AI-ROA-
        elif number_of_hints < len(word) // 2: # have we exhausted all hint
            hint = random.choice(hint_indices)
            print('letter', hint + 1, 'is', word[hint])
            hint_indices.remove(hint)
            print('remaining hint indices:', hint_indices)
            number_of_hints += 1
        else:
            print("You've already had all the hints you're gonna get!")
    else:
        # if any letter in the response is not in the guess, then tell them
        # for each letter in the response:
        #    if the letter is NOT in the word:
        #.      'this letter is not in the word'
        for letter in response:
            if letter not in word:
                print(letter, 'is NOT in the word, I will not count this a guess')
                break
        else: # only run IF we did not BREAK
            guesses += 1
            print('Incorrect guess!')
