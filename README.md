#include <iostream>
#include <vector>
#include <cstdlib>
#include <ctime>

using namespace std;

// Define card ranks and suits
const string ranks[] = {"2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K", "A"};
const string suits[] = {"Hearts", "Diamonds", "Clubs", "Spades"};

// Function to shuffle the deck
void shuffleDeck(vector<string> &deck) {
    int n = deck.size();
    for (int i = n - 1; i > 0; i--) {
        int j = rand() % (i + 1);
        swap(deck[i], deck[j]);
    }
}

// Function to deal two cards to each player
void dealCards(vector<string> &deck, vector<string> &player1Hand, vector<string> &player2Hand) {
    for (int i = 0; i < 2; i++) {
        player1Hand.push_back(deck.back());
        deck.pop_back();
        player2Hand.push_back(deck.back());
        deck.pop_back();
    }
}

// Function to initiate a betting round and get player decisions
void startBettingRound(int &player1Chips, int &player2Chips, int &pot, int &player1Bet, int &player2Bet) {
    cout << "Betting round started." << endl;
    
    // Player 1's turn
    cout << "Player 1's turn. Enter 'check' to check or 'bet' to bet (in chips): ";
    string player1Decision;
    cin >> player1Decision;
    player1Bet = 0; // Initialize player1Bet

    if (player1Decision == "bet") {
        cout << "Enter your bet amount (in chips): ";
        cin >> player1Bet;
        
        // Update player1Chips, pot, and player2's decision based on the bet
        player1Chips -= player1Bet;
        pot += player1Bet;
    }
    
    // Player 2's turn
    cout << "Player 2's turn. Enter 'check', 'call', or 'bet' (in chips): ";
    string player2Decision;
    cin >> player2Decision;
    player2Bet = 0; // Initialize player2Bet

    if (player2Decision == "bet") {
        cout << "Enter your bet amount (in chips): ";
        cin >> player2Bet;
        
        // Update player2Chips, pot, and player1's decision based on the bet
        player2Chips -= player2Bet;
        pot += player2Bet;
    } else if (player2Decision == "call") {
        // Player 2 chooses to call Player 1's bet
        player2Chips -= player1Bet; // Calling Player 1's bet
        pot += player1Bet; // Adding the call amount to the pot
    }
}

int main() {
    // Initialize random seed
    srand(time(0));

    // Create and shuffle the deck
    vector<string> deck;
    for (const string &rank : ranks) {
        for (const string &suit : suits) {
            deck.push_back(rank + " of " + suit);
        }
    }
    shuffleDeck(deck);

    // Initialize player hands and chip counts
    vector<string> player1Hand;
    vector<string> player2Hand;
    int player1Chips = 100 * 100; // 100 BB
    int player2Chips = 100 * 100; // 100 BB
    int pot = 0;
    int player1Bet = 0;
    int player2Bet = 0;

    // Deal two cards to each player
    dealCards(deck, player1Hand, player2Hand);

    // Display player hands and chip counts
    cout << "Player 1's Hand: " << player1Hand[0] << " and " << player1Hand[1] << endl;
    cout << "Player 2's Hand: " << player2Hand[0] << " and " << player2Hand[1] << endl;
    cout << "Player 1's Chips: " << player1Chips << endl;
    cout << "Player 2's Chips: " << player2Chips << endl;

    // Start the first betting round
    startBettingRound(player1Chips, player2Chips, pot, player1Bet, player2Bet);

    // Display updated chip counts and pot size
    cout << "Player 1's Chips after betting: " << player1Chips << endl;
    cout << "Player 2's Chips after betting: " << player2Chips << endl;
    cout << "Pot size after betting: " << pot << endl;

    // Deal the flop (3 community cards)
    for (int i = 0; i < 3; i++) {
        cout << "Dealing flop card " << (i + 1) << ": " << deck.back() << endl;
        deck.pop_back();

