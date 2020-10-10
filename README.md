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

The code used to provide this data was written in the Python coding language edited using Visual Studio Code. After all variables were defined, the first thing the script does is create for loop that loops throigh all rows of data in the original CSV data sheet. A counter was then defined that determines the total number of votes. Next, the candidate names were retrieved using their index in the worksheet and a decision statement was used to build a list of the candidates voted for. Every time a candidate's name appeared, a vote was added in a dictionary that contains the candidates name and corresponding votes. This process was then repeated to build a dictionary of counties and their corresponding votes. 

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

In conclusion, the script was an efficient and logical way to determine that provide a business proposal to the election commission on how this script can be used—with some modifications—for any election. Give at least two examples of how this script can be modified to be used for other elections.
