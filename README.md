# Hemingway
Arrays of Structs, Dynamic Memory Allocation  -C++


# Code with Comments


//libraries
#include <iostream>
#include <fstream>
#include <string>

using namespace std;


//Struct
struct Word{
    string word;
    int num;
};


//Function for returning true/false based on list of 50 most common words
bool MostCommonWords(string testword)
{
    string CommonWordArray[50] = {"the", "you", "one", "be", "do", "all", "to", "at", "would", "of",

"this", "there", "and", "but", "their", "a", "his", "what", "in", "by", "so", "that",

"from", "up", "have", "they", "out", "i", "we", "if", "it", "say", "about", "for", "her",

"who", "not", "she", "get", "on", "or", "which", "with", "an", "go", "he", "will", "me", "as",

"my"};
        for (int k = 0; k < 50; k++)
        {
            if(CommonWordArray[k] == testword)
            {
                return true;
            }
        }
    return false;
}


//Bubble Sort function sorting array from greatest count to least
void BubbleSort(Word WordArray[], int length)
{
    bool swap_occurred = true;
    Word temp;

    for(int i = 0; i < length && swap_occurred == true; i++)
    {
        swap_occurred = false;

        for(int j = 0; j < length - 1; j++)
        {
            if(WordArray[j].num < WordArray[j+1].num)
            {
                //swapping using temp holder
                temp = WordArray[j];
                WordArray[j] = WordArray[j+1];
                WordArray[j+1] = temp;

                swap_occurred = true;
            }
        }
    }

}


//main
int main(int argc, char*argv[])
{

int counter_newWords = 0;
int length = 100;
int numArrayDoubled = 0;
int WordCount = 0;

//Array pointing to Struct of length length
Word * WordArray= new Word[length];

//read in file
ifstream myFile(argv[1]); //(argv[1])
    if (myFile)
    {
        string slice;
        //read in file "slicing" each word into a string stored in slice using (myFile>>slice) built-in command
        while (myFile >> slice)
        {
            //call common words function
            if (MostCommonWords(slice) == false)
            {
            //count for number of unique non-common words
            WordCount++;
            //evaluate word as a new word (=true)
            bool newWord = true;
            //cout<<slice<<endl;
            //cout<<length<<endl;

            //evaluate if there is a match in the array to the incoming word, if so increase num by ++ at that location in the array
            //will prevent
            for (int i=0; i < counter_newWords; i++)
            {
                if(WordArray[i].word == slice)
                {
                    //cout<<"Match"<<endl;
                    WordArray[i].num++;
                    newWord = false;
                }
            }

            //evaluate if the incoming word (slice) is a new word (newWord == true)
            if (newWord == true)
            {
                //doubling array if needed
                if (counter_newWords == (length))
                {

                    numArrayDoubled++;

                    length = length * 2;
                    Word * newArray = new Word[length];

                    for(int k = 0; k < counter_newWords; k++)
                    {
                        newArray[k] = WordArray[k];
                    }

                    delete [] WordArray;
                    WordArray = newArray;
                }

                //adding to the array
                Word CurrentWord;
                CurrentWord.word = slice;
                CurrentWord.num = 1;
                WordArray[counter_newWords] = CurrentWord;

                //increment counter
                counter_newWords++;
            }
            }
        }

    }

//call BubbleSort function, passing through WordArray and WordArray's length (length)
BubbleSort(WordArray, length);

//generate desired outputs
    for(int k = 0; k < atoi(argv[2]); k++)
    {
        cout << WordArray[k].num << " - " << WordArray[k].word << endl;
    }
    cout << "#" << endl;
    cout << "Array doubled: " << numArrayDoubled << endl;
    cout << "#" << endl;
    cout << "Unique non-common words: " << counter_newWords << endl;
    cout << "#" << endl;
    cout << "Total non-common words: " << WordCount << endl;

    //return
    return 0;

//end main
}

//
//compile in terminal
//g++ -std=c++11 Assignment2.cpp -o Assignment2
//
//run argv[1],argv[2]
//./Assignment2 Hemingway_edit.txt 10
// or ./Assignment2 argv[1] argv[20
