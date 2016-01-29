# Introduction #

In Australian Voting the voters are asked to rank each candidate instead of selecting only one. So each ballot is the length of the number of candidates. The first position is the voters first choice, then second and so forth.  This is because to ensure a true election, only a candidate with explicitly more than 50 % of the votes can win. The process is more intricate than it appears at first. The process starts with an initial election, if no candidate is the clear majority a run off process starts. The minimum number of votes per candidate must be stored, this candidate OR candidates are losers.  The ballots who voted them as the primary winner must be re-examined to see who the next highest is preferred. These votes are then redistributed and the election process happens again. If at the very last iteration if there are multiple candidates with the same number of ballots (less than 50%), they are all printed out.

# Implementation #
Our runner method first calls the run method in Voting.h. In order to make our code more manageable this method only takes in the number of elections and pass the input to read where it parses the input per each election. Once our data structures are filled we call an eval method which controls the flow of the election when run offs are needed. The run off method returns either a positive integer, 0 or a -1. If a positive integer is returned, there is a majority winner and should be printed out. A 0 means there is a tie and a -1 means the loser votes need to be redistributed.  Eval will the then call moveBallots then try run off again until either a majority winner or a tie is reached between the remaining candidates.

# Details #
We chose not to make any of our own additional objects in attempt to lower the run time. The structures we do use are :

1. Vector of vector of lists named voteVector: the encasing vector represents the entire election

2. Each vector of lists represents the candidate, voteVector at 0 is empty because we wanted the indices on the ballots to match up directly to the candidates indices.

3. Each list of integers is a ballot. Instead of leaving the ballots as strings we pushed them on to a list because it was easier to iterate through when redistributing the votes.

4. We have a vector of booleans initialized to false to keep track of which candidates have been previously declared losers. Every time we move ballots, we check in the loser vector if the next highest candidate is a loser, if the candidate is a loser, we keep popping from the front of the list



# Testing #
We used a random number generator multiple numbers in Random.c++ which outputs a 1000 election test case.

## Issues Encountered ##
The biggest problem we encountered is that we did not initially account for ties in intermediary elections. Our first implementation could only handle clear cut winners. To fix this we kept track of the minimum vote count and maximum vote count at each iteration.  This was important for if at a certain iteration there were more than one candidate that received the lowest amount of votes.  We also had a vector of booleans that would keep track of previous losers.  This is important for when redistributing the losers ballots we needed to check that the ballot did not go to another loser declared at this or a previous iteration. The  maximum count is used to see if there is a tie. At first we tried having a vector of potential winners if we detected multiple candidates having the maximum vote but it was more inefficient than keeping the min and max integers and using a second pass.  The second pass checks the size of each candidates "bucket" to find those who tied.

The other most challenging part was the parsing the input without assertions our program would segfault improperly formed input. We spent a lot of time with off by one errors due to switching between << and getline for the input.


Sites Referenced:
http://stackoverflow.com/questions/3784812/terminating-multi-line-user-input
used in input to get the ballots