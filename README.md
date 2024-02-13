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
