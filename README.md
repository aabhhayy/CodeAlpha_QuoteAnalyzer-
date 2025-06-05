# CodeAlpha_QuoteAnalyzer-
#OVERALL CODE IS DONE ON GOOGLE COLLAB
#libraries used to scrap data from a website and then visualize it
pip install requests beautifulsoup4 pandas matplotlib seaborn textblob
# Website URL choose  (Example: Quotes website)
import requests
from bs4 import BeautifulSoup
import csv
url = 'http://quotes.toscrape.com/'

response = requests.get(url)
soup = BeautifulSoup(response.text, 'html.parser')

quotes = soup.find_all('div', class_='quote')

data = []
for quote in quotes:
    text = quote.find('span', class_='text').text
    author = quote.find('small', class_='author').text
    tags = [tag.text for tag in quote.find_all('a', class_='tag')]
    data.append([text, author, ', '.join(tags)])

# Code to save the content in (.CSV) fromat.
with open('quotes.csv', 'w', newline='', encoding='utf-8') as file:
    writer = csv.writer(file)
    writer.writerow(['Quote', 'Author', 'Tags'])
    writer.writerows(data)

print("Scraping complete and data saved to quotes.csv")
# Code to load CSV file data 
import pandas as pd

df = pd.read_csv('quotes.csv')

# Data summary
print(df.info())
print(df.describe())
print(df.head())

# Check missing values
print(df.isnull().sum())

# Unique authors count
print("Unique authors:", df['Author'].nunique())

# Use Basic statistics to analyze overall dataset condition
print(df['Tags'].value_counts().head(10))
import matplotlib.pyplot as plt
import seaborn as sns

# Author distribution plot
plt.figure(figsize=(10,6))
top_authors = df['Author'].value_counts().head(10)
sns.barplot(x=top_authors.index, y=top_authors.values, palette='viridis')
plt.title('Top 10 Authors by Number of Quotes')
plt.xticks(rotation=45)
plt.xlabel('Author')
plt.ylabel('Number of Quotes')
plt.show()

# Tags count plot (barplot for top 10 tags)
tags_series = df['Tags'].str.split(', ').explode()
top_tags = tags_series.value_counts().head(10)
plt.figure(figsize=(10,6))
sns.barplot(x=top_tags.index, y=top_tags.values, palette='rocket')
plt.title('Top 10 Tags')
plt.xticks(rotation=45)
plt.xlabel('Tag')
plt.ylabel('Count')
plt.show()
import matplotlib.pyplot as plt
import seaborn as sns

# Author distribution plot
plt.figure(figsize=(10,6))
top_authors = df['Author'].value_counts().head(10)
sns.barplot(x=top_authors.index, y=top_authors.values, palette='viridis')
plt.title('Top 10 Authors by Number of Quotes')
plt.xticks(rotation=45)
plt.xlabel('Author')
plt.ylabel('Number of Quotes')
plt.show()

# Tags count plot (barplot for top 10 tags)
tags_series = df['Tags'].str.split(', ').explode()
top_tags = tags_series.value_counts().head(10)
plt.figure(figsize=(10,6))
sns.barplot(x=top_tags.index, y=top_tags.values, palette='rocket')
plt.title('Top 10 Tags')
plt.xticks(rotation=45)
plt.xlabel('Tag')
plt.ylabel('Count')
plt.show()


















