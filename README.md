
# Cricket Data Analysis Project 🏏  

This project analyzes T20 Cricket World Cup data. The raw data was provided in JSON format, which was processed and transformed using Python and Pandas. The output is a set of cleaned and structured CSV files that are ready for analysis.  

---

## Project Features ✨  
- **Data Cleaning:** Fixed encoding issues, missing values, and inconsistencies in the raw JSON data.  
- **Data Transformation:** Mapped match IDs and established relationships between datasets.  
- **Export to CSV:** Saved the processed data as CSV files for further analysis.  

---

## Dataset Overview 📊  
### Files Used  
1. **t20_wc_match_results.json:** Match summaries.  
2. **t20_wc_batting_summary.json:** Batting performance details.  
3. **t20_wc_bowling_summary.json:** Bowling performance details.  
4. **t20_wc_player_info.json:** Player information.  

### Processed Outputs  
- **dim_match_summary.csv:** Match details and IDs.  
- **fact_batting_summary.csv:** Batting statistics with match IDs.  
- **fact_bowling_summary.csv:** Bowling statistics with match IDs.  
- **dim_players_no_images.csv:** Player information with cleaned names.  

---

## How It Works 🚀  

### Prerequisites  
- Python 3.x  
- Pandas Library  

Install the required libraries:  
```bash  
pip install pandas  
```  

### Data Processing Steps  

#### 1. Match Summary  
Extract match information and create a relational match ID dictionary.  
```python  
with open('t20_json_files/t20_wc_match_results.json') as f:  
    data = json.load(f)  

df_match = pd.DataFrame(data[0]['matchSummary'])  
df_match.rename({'scorecard': 'match_id'}, axis=1, inplace=True)  

match_ids_dict = {}  
for index, row in df_match.iterrows():  
    key1 = row['team1'] + ' Vs ' + row['team2']  
    match_ids_dict[key1] = row['match_id']  

df_match.to_csv('T20/dim_match_summary.csv', index=False)  
```  

#### 2. Batting Summary  
Transform batting records, clean player names, and export to CSV.  
```python  
with open('t20_json_files/t20_wc_batting_summary.json') as f:  
    data = json.load(f)  

all_records = []  
for rec in data:  
    all_records.extend(rec['battingSummary'])  

df_batting = pd.DataFrame(all_records)  
df_batting['match_id'] = df_batting['match'].map(match_ids_dict)  
df_batting.to_csv('T20/fact_batting_summary.csv', index=False)  
```  

#### 3. Bowling Summary  
Extract and structure bowling data linked to match IDs.  
```python  
with open('t20_json_files/t20_wc_bowling_summary.json') as f:  
    data = json.load(f)  

all_records = []  
for rec in data:  
    all_records.extend(rec['bowlingSummary'])  

df_bowling = pd.DataFrame(all_records)  
df_bowling['match_id'] = df_bowling['match'].map(match_ids_dict)  
df_bowling.to_csv('T20/fact_bowling_summary.csv', index=False)  
```  

#### 4. Player Information  
Clean player names and save player details.  
```python  
with open('t20_json_files/t20_wc_player_info.json') as f:  
    data = json.load(f)  

df_players = pd.DataFrame(data)  
df_players['name'] = df_players['name'].apply(lambda x: x.replace('â€', '').replace('†', '').replace(' ', ''))  
df_players.to_csv('T20/dim_players_no_images.csv', index=False)  
```  

---

## Directory Structure 📂  
```
project/  
│  
├── t20_json_files/  
│   ├── t20_wc_match_results.json  
│   ├── t20_wc_batting_summary.json  
│   ├── t20_wc_bowling_summary.json  
│   ├── t20_wc_player_info.json  
│  
├── T20/  
│   ├── dim_match_summary.csv  
│   ├── fact_batting_summary.csv  
│   ├── fact_bowling_summary.csv  
│   ├── dim_players_no_images.csv  
│  
└── cleanning.ipynb  
```  

---

## Technologies Used 🛠  
- **Python**: Primary programming language.  
- **Pandas**: Data manipulation and cleaning.  
- **JSON**: Input file format.  
- **CSV**: Output file format.  

---

## How to Use 📌  
1. Clone the repository:  
   ```bash  
   git clone <repo-url>  
   ```  
2. Run the Python scripts in sequence or use the provided Jupyter notebook `cleanning.ipynb`.  
3. Outputs will be generated in the `T20/` directory.  

---

## Contributors 🙌  
Feel free to fork the repository and contribute!  

---

## License 📜  
This project is licensed under the MIT License.  
