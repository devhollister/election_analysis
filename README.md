# Election Analysis

## Overview of Election Audit: 

The following is an audit of election data taken from a U.S. congressional district during a recent congressional election performed with the intention of certifying the results of the election. The district is comprised of three counties: Arapahoe, Jefferson, and Denver, and a total of three candidates, Charles Casper Stockham, Diana DeGette, and Raymon Anthony Doane, ran in the election. The total number of votes cast, a breakdown of votes for each of the three candidates, a determination of the winning candidate, and a breakdown of votes by county (including the county with the largest vote turnout) were determined in the analysis. The analysis was performed using the Python programming language in order to allow the auditing process to become automated and easily applied to future elections, congressional and beyond. 

## Election-Audit Results:
### Vote Breakdown:

The results of the anaylsis were as follows:

* The total number of votes cast in this congressional election was 369,711.
* The breakdown of votes by county were as follows:
  * Jefferson: 10.5% (38,855 votes)
  * Denver: 82.8% (306,055 votes)
  * Arapahoe: 6.7% (24,801votes)
* The county with the largest number of votes was Denver County. 
* The breakdown of votes by county were as follows:
  * Charles Casper Stockham: 23.0% (85,213)
  * Diana DeGette: 73.8% (272,892)
  * Raymon Anthony Doane: 3.1% (11,606)
* Diana DeGette won the election with 272,892 votes (73.8% of the total votes)

### Code Breakdown:

The code used to provide this data was written in the Python coding language edited using Visual Studio Code. As seen below, the script first loads the .csv file election_results, writes the output to a .txt file and defines the variables used later in the script. Then, the script reads the .csv file, skipping the header row.

```# Add our dependencies.
import csv
import os

# Assign a variable to load a file from a path.
file_to_load = os.path.join("Resources", "election_results.csv")
# Assign a variable to save the file to a path.
file_to_save = os.path.join("analysis", "election_results.txt")

# Initialize a total vote counter.
total_votes = 0

# Candidate Options and candidate votes.
candidate_options = []
candidate_votes = {}

# 1: Create a county list and county votes dictionary.
county_options = []
county_votes = {}

# Track the winning candidate, vote count and percentage
winning_candidate = ""
winning_count = 0
winning_percentage = 0

# 2: Track the largest county and county voter turnout.
county_with_largest_turnout = ""
largest_county_turnout = 0
largest_vote_percentage_by_county = 0

# Read the csv and convert it into a list of dictionaries
with open(file_to_load) as election_data:
    reader = csv.reader(election_data)
# Read the header
    header = next(reader)
```
After all variables were defined, the first thing the script does is create for loop that loops through all rows of data in the original .csv data sheet. A counter was then defined that determines the total number of votes. Next, the candidate names were retrieved using their index in the worksheet and a decision statement was used to build a list of the candidates voted for. Every time a candidate's name appeared, a vote was added in a dictionary that contains the candidates name and corresponding votes. This process was then repeated to build a dictionary of counties and their corresponding votes. 

    # For each row in the CSV file.
    for row in reader:

        # Add to the total vote count
        total_votes = total_votes + 1

        # Get the candidate name from each row.
        candidate_name = row[2]

        # 3: Extract the county name from each row.
        county_name = row[1]

        # If the candidate does not match any existing candidate add it to
        # the candidate list
        if candidate_name not in candidate_options:

            # Add the candidate name to the candidate list.
            candidate_options.append(candidate_name)

            # And begin tracking that candidate's voter count.
            candidate_votes[candidate_name] = 0

        # Add a vote to that candidate's count
        candidate_votes[candidate_name] += 1

        # 4a: Write a decision statement that checks that the
        # county does not match any existing county in the county list.
        if county_name not in county_options:

            # 4b: Add the existing county to the list of counties.
            county_options.append(county_name)

            # 4c: Begin tracking the county's vote count.
            county_votes[county_name] = 0

        # 5: Add a vote to that county's vote count.
        county_votes[county_name] += 1
        
Next, a series of calculations were performed and another decision statement was used to determined the county with the most votes. 

    # 6a: Write a repetition statement to get the county from the county dictionary.
    for county_name in county_votes:

        # 6b: Retrieve the county vote count.
        votes_by_county = county_votes.get(county_name)

        # 6c: Calculate the percent of total votes for the county.
        vote_percentage_by_county = float(votes_by_county) / float(total_votes) * 100
        county_results = (
            f"{county_name}: {vote_percentage_by_county:.1f}% ({votes_by_county:,})\n")

         # 6d: Print the county results to the terminal.
        print(county_results)

         # 6e: Save the county votes to a text file.
        txt_file.write(county_results)

         # 6f: Write a decision statement to determine the winning county and get its vote count.
        if (votes_by_county > largest_county_turnout) and (vote_percentage_by_county > largest_vote_percentage_by_county):
            largest_county_turnout = votes_by_county
            county_with_largest_turnout = county_name
            largest_vote_percentage_by_county = vote_percentage_by_county
            
This process was then repeated to find analyze the votes by candidate and determine the winner of the election.

    # Save the final candidate vote count to the text file.
    for candidate_name in candidate_votes:

        # Retrieve vote count and percentage
        votes = candidate_votes.get(candidate_name)
        vote_percentage = float(votes) / float(total_votes) * 100
        candidate_results = (
            f"{candidate_name}: {vote_percentage:.1f}% ({votes:,})\n")

        # Print each candidate's voter count and percentage to the
        # terminal.
        print(candidate_results)

        #  Save the candidate results to our text file.
        txt_file.write(candidate_results)

        # Determine winning vote count, winning percentage, and candidate.
        if (votes > winning_count) and (vote_percentage > winning_percentage):
            winning_count = votes
            winning_candidate = candidate_name
            winning_percentage = vote_percentage

## Election-Audit Summary:

In conclusion, the script was an efficient and logical way to determine that Diana DeGette won the election with 272,892 votes and that Denver County had the largest voter turnout (306,055 votes). With a few modifications, this script can be used to anaylize the results of any election, as the list of candidates and the voting districts are built by the script itself while reading the data sheet supplied to it. For exmaple, for a mayoral election the script would only have to change from referencing county data to referencing city voting districts. Furthermore, the script can be scaled up by adding more identical decision statements within the code. In a nationwide election, for example, in addition to having county data, another decision statement could be added to build a dictionary of the number of votes for each state. The operational power of Python also makes any number of analysis possible, for example, finding the two candidates with the most votes, as happens in a primary race, would be accomplished simply by altering the final decision statement at the end of the script. 
