## HTTP log analysis using Splunk
This project simulates analysing http log to find things like top endpoints which generating web traffic, total number of server errors, finding suspicious user-agents to find possible scripted attacks, etc...

## Tools Used
- Splunk enterprise
- Kali linux

## Steps Performed
- Imported http log file to splunk
- Analysed fields
- Used SPL (Search Processing Language) to perform tasks like
  - Find the top 10 endpoints generating web traffic
    - SPL query used:
    ```bash
    source="http_logs.json" host="kali" sourcetype="_json" 
    | stats count by "id.orig_h" 
    | sort -count
    | head 10
    ```

  - Count the number of server errors (5xx) observed
    - SPL query used:
    ```bash
    source="http_logs.json" host="kali" sourcetype="_json" status_code=5**
    | stats count as server_errors
    ```
  - Identify User-Agents associated with possible scripted attacks
    - SPL query used:
    ```bash
    source="http_logs.json" host="kali" sourcetype="_json" user_agent IN ("sqlmap/1.5.1", "curl/7.68.0", "python-requests/2.25.1", "botnet-checker/1.0")
    | stats count by user_agent
    ```
  - Find large file transfers (greater than 500 KB)
    - SPL query used:
    ```bash
    source="http_logs.json" host="kali" sourcetype="_json" resp_body_len>500000
    | table ts "id.orig_h" "id.resp_h" uri resp_body_len
    | sort -resp_body_len
    ```

## Screenshots
<img width="1920" height="1020" alt="Screenshot 2025-10-19 125747" src="https://github.com/user-attachments/assets/862de170-49c0-4c11-8b9e-1dc232c9fa4d" />
<img width="1920" height="1020" alt="Screenshot 2025-10-19 130126" src="https://github.com/user-attachments/assets/c4de24f3-6cdc-4262-b403-a453c3066c76" />
<img width="1920" height="1020" alt="Screenshot 2025-10-19 131220" src="https://github.com/user-attachments/assets/6901c28a-abd6-4f14-9553-99eca6edf71d" />
<img width="1920" height="1020" alt="Screenshot 2025-10-19 131735" src="https://github.com/user-attachments/assets/0bad276f-d738-4fb6-b5d1-02af975aaf35" />

## Outcome
Successfully analysed http log file using SPL.

## Note
This was a self-simulated lab exercise for learning and practicing.
