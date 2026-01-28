# Log Analysis Commands

These commands are for quickly extracting, filtering, and analyzing logs directly from the command line.  
Each example assumes a generic space-delimited log file — adjust field numbers or delimiters for your logs.

---

## 1️⃣ Count occurrences of unique values
```bash
awk '{print $3}' file.log | sort | uniq -c | sort -nr


Meaning:

awk '{print $3}' file.log → Select column 3 (e.g., an IP, domain, username).

sort → Sort the values alphabetically.

uniq -c → Count how many times each unique value occurs.

sort -nr → Sort numerically in reverse (highest count first).

Use case: Find the top IPs, domains, or usernames in a log.

2️⃣ Search for a keyword
grep "keyword" file.log


Meaning:

Searches the file for lines containing keyword.

Outputs the entire matching line.

Use case: Find all DELETE requests in HTTP logs.

3️⃣ Search (case-insensitive)
grep -i "keyword" file.log


Meaning:

Same as grep, but ignores case.

Matches "Delete", "DELETE", "delete".

Use case: Find terms regardless of capitalization.

4️⃣ Exclude lines with a keyword
grep -v "keyword" file.log


Meaning:

Outputs all lines except those containing the keyword.

Use case: Remove noise from logs (e.g., exclude known benign IPs).

5️⃣ Extract lines between two timestamps
awk '$1 >= "2025-08-10T12:00:00" && $1 <= "2025-08-10T13:00:00"' file.log


Meaning:

$1 is the first column (timestamp in this example).

>= and <= compare timestamps.

Only outputs lines between the given time range.

Use case: Focus on logs for a specific incident window.

6️⃣ Count matching lines
grep "DELETE" http.log | wc -l


Meaning:

grep "DELETE" http.log → Get lines with DELETE requests.

wc -l → Count the number of lines output.

Use case: Count how many DELETE requests occurred.

7️⃣ Show only specific columns
awk '{print $1, $3, $5}' file.log


Meaning:

$1 = column 1, $3 = column 3, $5 = column 5.

Prints them with spaces between.

Use case: Extract timestamp, source IP, and destination port.

8️⃣ Use tab as a delimiter
awk -F'\t' '{print $8}' file.log


Meaning:

-F'\t' sets the delimiter to a tab.

$8 → Prints the 8th column.

Use case: Zeek logs or CSV files where values are tab-delimited.

9️⃣ Find top N results
awk '{print $3}' file.log | sort | uniq -c | sort -nr | head -n 10


Meaning:

Same as command #1, but head -n 10 shows only the top 10 results.

Use case: Top 10 most requested URLs, IPs, or users.

🔟 Chain multiple filters
awk '{print $8}' http.log | grep -w DELETE | sort | uniq -c | sort -nr


Meaning:

$8 → Extracts the URI/path column.

grep -w DELETE → Filters only lines with "DELETE" as a whole word.

sort | uniq -c | sort -nr → Counts and sorts by frequency.

Use case: See which endpoints are targeted by DELETE requests.

📌 How to Find Column Numbers in Logs

Before running awk '{print $3}' ... you need to know which column contains the data you want.

1. Use head to preview
head -n 5 file.log


Meaning: Shows the first 5 lines of the log so you can see the structure.

2. Use awk to print all columns with numbers
awk '{for (i=1;i<=NF;i++) print "Column " i ": " $i}' file.log | head -n 20


Meaning:

NF = Number of Fields (columns) in each line.

Prints each column number and its value.

The head at the end limits output so you don’t flood the screen.

3. Look for delimiters

Logs may be:

Space-delimited → default for awk and many syslog formats.

Tab-delimited → Zeek logs, some CSVs (-F'\t' in awk).

Comma-delimited → CSV files (-F',' in awk).

Pipe-delimited → (-F'|' in awk).

4. Example log with column meanings
2025-08-10T12:30:45Z 192.168.1.10 10.0.0.5 TCP 443 ALLOW Mozilla/5.0
|   |               |             |        |   |   |     |
|   |               |             |        |   |   |     └── Column 7: User-Agent
|   |               |             |        |   |   └──────── Column 6: Action
|   |               |             |        |   └─────────── Column 5: Destination Port
|   |               |             |        └────────────── Column 4: Protocol
|   |               |             └─────────────────────── Column 3: Destination IP
|   |               └──────────────────────────────────── Column 2: Source IP
|   └──────────────────────────────────────────────────── Column 1: Timestamp
