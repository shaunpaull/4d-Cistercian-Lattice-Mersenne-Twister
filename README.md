# 4d-Cistercian-Lattice-Mersenne-Twister
Mersenne Twister'

//
#include <iostream>
#include <vector>
#include <random>

using namespace std;

// This function creates a 4D lattice.
vector<vector<vector<vector<unsigned int>>>> createLattice(int width, int height, int depth, int time) {
  // Create a random number generator.
  random_device rd;
  mt19937 gen(rd());
  uniform_int_distribution<unsigned int> distribution(0, 1114111); // Maximum Unicode code point

  // Create the lattice structure.
  vector<vector<vector<vector<unsigned int>>>> lattice(width, vector<vector<vector<unsigned int>>>(height, vector<vector<unsigned int>>(depth, vector<unsigned int>(time))));

  // Fill the lattice with random Unicode symbols.
  for (int i = 0; i < width; i++) {
    for (int j = 0; j < height; j++) {
      for (int k = 0; k < depth; k++) {
        for (int l = 0; l < time; l++) {
          lattice[i][j][k][l] = distribution(gen);
        }
      }
    }
  }

  return lattice;
}

// This function encrypts a message using the 4D lattice.
string encryptMessage(const string& message, const vector<vector<vector<vector<unsigned int>>>>& lattice) {
  // Convert the message to a sequence of Unicode symbols.
  vector<unsigned int> symbols;
  for (char c : message) {
    symbols.push_back(static_cast<unsigned int>(c));
  }

  // Encrypt the symbols using the 4D lattice.
  for (int i = 0; i < symbols.size(); i++) {
    unsigned int index1 = symbols[i] % lattice.size();
    unsigned int index2 = symbols[i] / lattice.size() % lattice[0].size();
    unsigned int index3 = symbols[i] / lattice.size() / lattice[0].size() % lattice[0][0].size();
    unsigned int index4 = symbols[i] / lattice.size() / lattice[0].size() / lattice[0][0].size() % lattice[0][0][0].size();

    cout << "Indices: " << index1 << ", " << index2 << ", " << index3 << ", " << index4 << endl;

    symbols[i] = lattice[index1][index2][index3][index4];
  }

  // Convert the encrypted symbols back to a string.
  string encryptedMessage;
  for (unsigned int symbol : symbols) {
    encryptedMessage += static_cast<char>(symbol);
  }

  return encryptedMessage;
}

// This function decrypts a message that was encrypted using the 4D lattice.
string decryptMessage(const string& encryptedMessage, const vector<vector<vector<vector<unsigned int>>>>& lattice) {
  // Convert the encrypted message to a sequence of Unicode symbols.
  vector<unsigned int> symbols;
  for (char c : encryptedMessage) {
    symbols.push_back(static_cast<unsigned int>(c));
  }

  // Decrypt the symbols using the 4D lattice.
  for (int i = 0; i < symbols.size(); i++) {
    unsigned int index1 = symbols[i] % lattice.size();
    unsigned int index2 = symbols[i] / lattice.size() % lattice[0].size();
    unsigned int index3 = symbols[i] / lattice.size() / lattice[0].size() % lattice[0][0].size();
    unsigned int index4 = symbols[i] / lattice.size() / lattice[0].size() / lattice[0][0].size() % lattice[0][0][0].size();

    cout << "Indices: " << index1 << ", " << index2 << ", " << index3 << ", " << index4 << endl;

    symbols[i] = lattice[index1][index2][index3][index4];
  }

  // Convert the decrypted symbols back to a string.
  string decryptedMessage;
  for (unsigned int symbol : symbols) {
    decryptedMessage += static_cast<char>(symbol);
  }

  return decryptedMessage;
}

int main() {
  // Create a 4D lattice.
  const int width = 100;
  const int height = 100;
  const int depth = 100;
  const int time = 100;
  vector<vector<vector<vector<unsigned int>>>> lattice = createLattice(width, height, depth, time);

  // Encrypt a message.
  string message = "the snakes the rats, the cats, the DAWGS, to shine to shine, i HUSTLE T0 FLex, i make thE dEVil Go WEAK the Knees.";
  string encryptedMessage = encryptMessage(message, lattice);

  // Decrypt the message.
  string decryptedMessage = decryptMessage(encryptedMessage, lattice);

  // Print the encrypted and decrypted messages.
  cout << "Encrypted message: " << encryptedMessage << endl;
  cout << "Decrypted message: " << decryptedMessage << endl;

  return 0;
}
