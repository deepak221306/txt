import requests
import json
 
jira_url = 'https://deepakkhushcool.atlassian.net/'
bearer_token ="ATATT3xFfGF0IjoVSOSdXtpeTWAQPrJBYRBaBh5Dy5j2yoPfkayWFB02_3VyPOsuM8QEkLtYL1mvHj_V5RNIWUBYzmzXc27uxaYZ1hic70IqWQcvPV55FlgkPhxo3LIQyJLQwREN_M64wTc1GIIZQSgPWFxH7Zoddj-K-BSVfhVwpFdT_h8U7sw=98443D32"
headers = {'Authorization': 'Bearer ' + bearer_token, "Content-Type": "application/json"}
 
 
def api_op(req_api):
    api_response = requests.request("GET", req_api, headers={'Authorization': 'Bearer '+ bearer_token},verify=False)
    api_response_to_print = json.dumps(json.loads(api_response.text),sort_keys=True,indent=4,separators=(",", ": "))
    api_response_data = json.loads(api_response_to_print)
    return api_response_data
 
main_issue_types_list = []
subtasks_issue_types_list = []
all_issue_types = []
 
instance_issuetypes_url = f"{jira_url}/rest/api/2/issuetype"
data = api_op(instance_issuetypes_url)
 
# for entry in data:
#     all_issue_types.append(entry.get('name'))
for entry in data:
    try:
        all_issue_types.append(entry.get('name'))
    except AttributeError:
        print(f"Error: {entry} is not a dictionary. It's a {type(entry)}")
    ### Below logic is to get different list : main issue types and sub task issue types. To get it uncomment and run the script
    '''if entry.get('subtask'):
        subtasks_issue_types_list.append(entry.get('name'))
    else:
        main_issue_types_list.append(entry.get('name'))'''
 
print(all_issue_types)


other code #############################
import requests
import csv
 
# Jira server URL and credentials
jira_url = 'https://deepakkhushcool.atlassian.net'
username = 'deepakkhushcool25@gmail.com'
password = 'ATATT3xFfGF0lFKDhEEWl-pHQSL6mp4zUUq5vCbzXO3BC-zfNJDST3eEjHIphqILW9nP1kOmwH28znHWMkJ1b6SaAOZbu5JCBjIS7bonv7I59umVcGL5n5-OxzP_SkJdDbkoYZCCxVlMpuIy2Skn_DskYsUxL2BMPfA61TRkqY-nLHG20TDfzsg=27E960E1'

 
def get_unique_issue_types():
    try:
        # Construct URL for issue types endpoint
        url = f"{jira_url}/rest/api/2/issuetype"
 
        # Make API call to Jira
        response = requests.get(url, auth=(username, password),verify=False)
 
        # Check if request was successful
        if response.status_code == 200:
            # Parse JSON response
            data = response.json()
            # Extract unique issue types
            unique_issue_types = {issue_type['name'] for issue_type in data}
            return unique_issue_types
        else:
            print(f"Error: {response.status_code}, {response.text}")
            return None
 
    except Exception as e:
        print(f"An error occurred: {e}")
        return None
#  Saving data into csv file 
def save_to_csv(unique_issue_types, file_name='issue_types.csv'):
    try:
        with open(file_name, 'w', newline='') as file:
            writer = csv.writer(file)
            writer.writerow(['Issue Type'])
            for issue_type in unique_issue_types:
                writer.writerow([issue_type])
        print(f'Unique issue types saved to {file_name}')
    except Exception as e:
        print(f"An error occurred while saving to CSV: {e}")
 
# Get unique issue types from Jira
unique_issue_types = get_unique_issue_types()
 
if unique_issue_types:
    # Save unique issue types to CSV
    save_to_csv(unique_issue_types)
