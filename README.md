#csvkit-howto

Csvkit is a tool to convert, examine and work with (large) csv files. It removes the need to use Excel. Not only is csvkit faster because it uses the terminal, it can also handle a lot bigger csv files than Excel is capable of.


*Read ["How to be a Terminal Pro"](https://github.com/AJInteractive/terminalpro) to get to know the basics of the Terminal*

Most of the csvkit stuff is based on simple terminal commands, with some specific commands which are listed below

##INSTALLING PYTHON AND CSVKIT (http://docs.python-guide.org/en/latest/starting/install/osx/):

1. Install Python (https://www.python.org/downloads/)
2. sudo pip install csvkit

##Now onto csvkit/terminal magic:

1. in2csv [filename.xlsx] > [filename.csv] --> Converts Excel file into csv file
2. head [filename.csv] --> see the first ten lines of the csv file (use -n [number] to change ten lines to desired number). Preferable to csvlook actually, especially with bigger files
3. tail [filename.csv] --> Same as head, but for last ten lines
4. csvlook [filename.csv] -->  print csv file in the terminal, great for a first look of the file. Useful in combination with csvcut (see below) and head.
5. csvlook [filename] | head -->  print the first ten lines of csv file in the terminal
6. csvcut -n [filename.csv] --> print the indices and names of all the columns in the csv file
7. csvstat [filename.csv] --> get summary statistics on the csv file you're working on (most frequent values, longest value, shortest value etc). Useful for quick statistical overview
8. csvclean [filename.csv] --> cleans csv file of common syntax errors. Creates two files, one with the clean lines, one with the lines containing errors
9. csvstack [filename1.csv] [filename2.csv] > [newfilename.csv] --> 'Stack' two csv files on top of each other in one file

##Some more advanced stuff:

1. csvcut [filename.csv] -c 1,3,5 | csvlook  --> will take columns 1,3, and 5 and print them in the terminal using csvlook
2. csvcut [filename.csv] -c 1,3,5 > [newfilename.csv] --> will take columns 1,3, and 5 and put just these columns in new file
3. csvcut -l [filename.csv] > [newfilename.csv] --> Add line numbers to [filename.csv] and save this output in [newfilname.csv]
4. csvgrep [filename.csv] -c 1,2 -m "[searchterm]"  --> Search for rows containing [searchterm] in columns 1 and 2 in [filename.csv]
5. csvgrep [filename.csv] -c 1,4 -r "^R" --> Search for rows that have values that begin with the letter R in columns 1 and 4
6. csvgrep -c "ColumnName" -r "\d{3}-123-\d{4}" --> Search for rows that contain the following pattern: [ddd]–123-[dddd]
7. csvsort [filename.csv] -c [ColumnName/ColumnNumber]
8. csvjoin [filename.csv] [filename2.csv] > [newfilename.csv] --> Join two csv files into one new file. Uses 'inner' join, can use --outer --right or --left to choose which type of join you want (differences between them here: http://stackoverflow.com/questions/448023/what-is-the-difference-between-left-right-outer-and-inner-joins)

*Remember that you can pipe together different commands using the | command, so for example these all work:*

1. csvcut -c [ColumnName],[ColumnName2],[ColumnName3] [filename.csv] | csvstat --> will cut the three specified columns from [filename.csv] and do a csvstat on these three columns only
2. csvcut -c [ColumnName],[ColumnName2],[ColumnName3] [filename.csv] | csvlook | head --> will cut the three specified columns from [filename.csv] and present the first ten lines (head) in the terminal using csvlook
3. csvgrep -c 5 -m "[SearchQuery]" -i [filename].csv | csvcut -c 1,2,3,4,7,8,9 >[newfilname.csv] --> This will look for rows that DON'T have [Searchquery] in column 5 of [filename.csv]. Because of the -i flag (-i stands for inverse) csvgrep looks for all rows that don't have [SearchQuery] in column 5 will be selected.  Piping it into csvcut it will take columns 1,2,3,4,7,8,9 of the rows NOT containing [SearchQuery] and put these into [newfilename.csv]

##Next level things:

1. in2csv -f fixed -s [schema.csv] [messyfile.txt] > [fixedFile.csv] --> Convert [messyfile.txt] text file into a nicely usable csv file using a predetermined schema (in this case [schema.csv]). Create own schemas or use premade ones available on the internet.
2. csvformat -D "|" [filename.csv] > [newfilename.csv] --> csvformat is used to format csv files into a custom csv output format. In this case, the comma separated file will be turned into a pipe-delimited csv file
3. csvjson -k "[key]" -i 4 [filename.csv] --> Convert csv file into json file. -k indicates the key, -i is amount of indentation. All values in a column must be unique for this
4. csvjson --lat latitude --lon longitude --k slug --crs [CRS code] -i 4 [filename.csv] --> turns csv file into a GeoJSON file, needs long/lats. CRS is coordinate reference system which needs long/lats, different projection systems (EPSG:900913 for Google Maps)
5. csvpy [filename.csv] --> load the csv file into a Python shell, allowing for inspecting the data in any way you want using Python

##Csvkit and SQL databases:

1. csvsql -i postgresql [filename.csv] --> Will generate a statement in the Postgres dialect
2. createdb [dbname]
   csvsql --db postgresql:///[dbname] --insert [filename.csv] --> Create a SQL table and import a CSV into a database
3. createdb test
   csvsql --db postgresql:///[database] --insert [foldername/[filename].csv] --> Creates tables from all csv files in folder and imports the data into Postgres
4. sql2csv --db postgresql:///[database] --query "[query]" > extract.csv


##*MORE INFO ON DIFFERENT TYPES OF FLAGS ETC:*

[CSVkit.readthedocs.io](https://csvkit.readthedocs.io/en/0.9.1/cli.html)

#csvkit-howto
