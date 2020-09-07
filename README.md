# Pandas
- ## Libraries
	- ```python
		import pandas as pd
		```
	- ## Read files from the system
		- ```python
			# Read a csv
			df  = pd.read_csv('file_name.csv')
			# Read a excel
			dfe = pd.read_csv('file_name.xlsx')
			# Read a comma separated txt
			dft = pd.read_csv('file_name.txt, delimeter='\t')
			```
	- ## Get the top n number of rows
		- ```python
			df.head(n)
			```
	- ## Get the last n number of columns
		- ```python
			df.tail(n)
			```
	- ## get the headers
		- ```python
			df.columns
			```
	- ## splice the data frame
		- ```python
			df[0:11]
			```
	- ## display only the particular columns
		- ```python
			df['Name']
			```
	- ## print a number of selected columns
		- ```python
			df[['Name','Type 1','HP']]
			```
	- ## Iloc
		- ```python
			## get the first column
			df.iloc[0:5]
			## get the second column
			df.iloc[2]
			## get the second column and first row
			df.iloc[2.1]
			## splice both rows and columns
			df.iloc[0:4,4:11]
			```
	- ## iterrows
		- ```python
			for index,row in df.iterrows():
				display(index,row[['Name','HP']])
			```
	- ## loc
		- ```python
			## get the rows where type 1 == Fire
			df.loc[df['Type 1']=="Fire"]
			```
	- ## describe 
		- ```python
			#get the high level stats of your data
			#min, max, mean, std dev
			df.describe()
			```
	- ## Sort the data
		- ```python
			#sort based upon names first
			# and then the hp
			# in the ascending order
			df.sort_values(['Name','HP'],ascending=False)
			```
	- ## Making changes in the data
		- ```python
			#creates a new column
			df['Attack Potential']=
			df['Sp. Atk']+df['Attack']
			```
	- ## Print the data in the column order that you want to
		- ```python
			df[['Name','Attack Potential']][5:16]
			```
	- ## Drop a given column
		- ```python
			df = df.drop(columns=['Attack Potential'])
			```
	- ## Calculate the sum along the vertical axis
		- ```python
			df['Total']=df.iloc[:,4:10].sum(axis=1)
			```
	- ## Move columns using splicing (not recommended way)
		- ```python
			cols=list(df.columns)
			cols[-1]
			#single column will appear as a string
			[cols[-1]]
			#will appear as a list
			cols[0:4]
			#multiple columns will 
			#automatically appear as a list
			df = df[cols[1:4]+cols[-1]+cols[4:10]]
			#move multiple columns using splicing
			```
	- ## saving the data 
		- ```python
			dfe=df[0:31]
			dfe.to_csv('Modified_pokemons.csv',index=False)
			dfe.to_excel('Modified_pokemons.xlsx',index=False)
			```
	- ## Filtering the data
		- ```python
				#single condition
				df.loc[df['Type 1']=='Grass']
				#multiple conditions with and
				df.loc[(df['Type 1']=='Grass')&(df['Type 2]=='Poison')]
				#multiple conditions with or
				df.loc[(df['Type 1']=='Grass')|(df['Type 2'])=='Poison')]
				##multiple conditions with inequality
				df.loc[((df['Type 1']=='Green')|(df['Type 2']=='Poison'))&(df['HP']>80)]
			```
	- ## Resetting the index
		- ```python
			new_df=df.reset_index(drop=True)
			```
	- ## String contains
		- ```python
			#fetch all the pokemons which contain the word mega
			df.loc[df['Name'].str.contains('Mega')]
			```
	- ## Using the not sign
		- ```python
			# the ~ sign acts as not
			df.loc[~df['Name'].str.contains('Mega')]
			```
	- ## Regular expressions
		- ```python
			import re
			#flags=re.I ignores the case
			#regex = True signifies the we are using regulat expressions
			df.loc[df['Type 1'].str.contains('fire|grass',flags=re.I,regex=True)]
			#Names that start with pi and have any other alphabets afterwards
			df.loc[df['Name'].str.contains('^pi[a-z]+',flags=re.I,regex=True)]
			```
	- ## Conditional changes
		- ```python
			##set Type 1  to flamer where type 1 = fire
			df.loc[df['Type 1']==Fire,'Type 1']=='Flamer'
			##assign the same value to two columns  
			df.loc[df['HP']<70,['Type 1','Type 2']]="Test Stat"
			#assign different values to different columns
			df.loc[df['HP']<70,['Type 1','Type 2']]=['Grass','Poison']
			```
	- ## Aggregate statistics(groupby)
		- ```python
			#find the mean of all columns based upon the values of type 1
			df.groupby(['Type 1']).mean()
			#sort the values based upon defense in descending order
			df.groupby(['Type 1']).mean().sort_values('Defense',ascending=False)
			```
	- ## count column values horizontally
		- ```python
			#find the count of all the columns values horizontally 
			df.groupby(['Type 1']).count()
			#create a new column count with all values 1
			df['count']=1
			#count the number of counts based upon counts
			df.groupby(['Type 1']).count()['count']#count the number of type 1
			```
	- ## working with large amounts of data
		- ```python
			#load the data in chunks
			for dfc in pd.read_csv('pokemon_data.csv',chunksize=10):
				print('chunk dfc')
				display(dfc)
			```
	- ## performing the operations on the chunks of data and the performing operations on the results of these chunks
		- ```python
			#load the chunks of data into chunks
			chunks=pd.read_csv("pokemon_data.csv",chunksize=10)
			#save the results from chunks in the pieces
			pieces=[x.groupby(['Type 1']).mean() for x in chunks]
			#concatinate the pieces
			new_df=pd.concat(pieces)
			#get the mean of the concatenated data
			new_df.groupby(['Type 1']).mean()
			```
			