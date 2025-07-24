### 🔹 **AWK**

> "**AWK is a text-processing tool and a small programming language used to extract, format, and report data from text files or streams.** I often use it to filter specific columns, do arithmetic on fields, or generate formatted reports — especially when analyzing logs or CSVs."

✅ Example:
*"I used `awk '{print $2, $3}'` to extract name and age from a space-separated text file."*

---

### 🔹 **SED**

> "**SED stands for stream editor. It is used to search, replace, insert, or delete text in a file or stream.** I use it mostly for modifying config files or cleaning up large text data during automation scripts."

✅ Example:
*"I used `sed 's/8080/80/g'` to replace ports in configuration files during deployment."*

---

### 🔹 **GREP**

> "**GREP is used to search for patterns in text.** It filters lines that match a given regular expression. It's very handy for searching logs, configs, or any text file."

✅ Example:
*"I use `grep 'ERROR' /var/log/app.log` to quickly check for error messages in logs."*

---

### 🔹 **CUT**

> "**CUT is used to extract specific fields or characters from each line of a text file.** It's fast and useful when dealing with structured logs or CSV files."

✅ Example:
*"I used `cut -d':' -f2` to extract the second field from colon-separated output like `/etc/passwd`."*

---

### 👉 Quick One-Line Comparison:

> "**AWK** is for field-based processing,
> **SED** is for stream editing,
> **GREP** is for searching,
> **CUT** is for extracting columns or characters."

---

### `data.txt`:

```
1 John  25 Developer
2 Alice 30 Manager
3 Bob   28 Tester
4 Eve   22 Intern
```

---

### 🔹 `AWK` Examples:

```bash
# 1. Print all lines
awk '{print}' data.txt

# 2. Print only the names (2nd column)
awk '{print $2}' data.txt

# 3. Print name and age where age > 25
awk '$3 > 25 {print $2, $3}' data.txt

# 4. Calculate average age
awk '{sum += $3} END {print "Average age:", sum/NR}' data.txt

# 5. Print formatted output
awk '{printf "Name: %s, Age: %s\n", $2, $3}' data.txt
```

---

### 🔹 `SED` Examples:

```bash
# 1. Replace 'Developer' with 'Engineer'
sed 's/Developer/Engineer/' data.txt

# 2. Delete lines that contain 'Intern'
sed '/Intern/d' data.txt

# 3. Add '[EMPLOYEE]' at the start of each line
sed 's/^/[EMPLOYEE] /' data.txt

# 4. Insert a new line before line 3
sed '3i\ID Name Age Role' data.txt

# 5. Replace multiple spaces with a single space
sed 's/  */ /g' data.txt
```

---

### 🔹 `GREP` Examples:

```bash
# 1. Search for lines containing 'Manager'
grep 'Manager' data.txt

# 2. Case-insensitive search for 'alice'
grep -i 'alice' data.txt

# 3. Show line numbers with matches
grep -n 'Bob' data.txt

# 4. Invert match (print lines NOT containing 'Developer')
grep -v 'Developer' data.txt

# 5. Search lines starting with number 1
grep '^1' data.txt
```

---

### 🔹 `CUT` Examples:

```bash
# 1. Cut 2nd field (name) based on space delimiter
cut -d' ' -f2 data.txt

# 2. Show first and third fields (name and age)
cut -d' ' -f2,3 data.txt

# 3. Cut only first 10 characters of each line
cut -c1-10 data.txt
```

---
