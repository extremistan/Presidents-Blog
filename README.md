# Political Parties Blog

All of our analysis and results can be found in the blog on our [website](https://extremistanresearch.com). In this README, we will broadly explain our code, as   well as our thought process in our methodology. 
  
We used 2 python programs to gather the data for our blog. This was neccesaary because we wanted two different result sets, one with the percent change for each individual president, and one with the percent change by political party. While we could have done this in one program, we thought it would be more clear and concise to do it seperately.


## Programs

Both programs are very similar, so I will explain them together first, and then the individual differences after. 

For our data, we used a study at Yale (linked [here](http://www.econ.yale.edu/~shiller/data.htm)). We started off both programs with a dictionary of each president and their years served, as shown below. By using this, we could loop through our program and gather all the data on every president in one run through, instead of having to change the parameters and run the program multiple times.
```
president_dict = {"1877-1880":["R", "Hayes"], "1881-1884":["R", "Arthur"], "1885-1888":["D", "Cleveland 1st"], "1889-1892":["R", "Harrison"],
                    "1893-1896":["D", "Cleveland 2nd"], "1897-1900":["R", "McKinley"], "1901-1908":["R", "T Roosevelt"], "1909-1912":["R", "Taft"],
                    "1913-1920":["D", "Wilson"], "1921-1922":["R", "Harding"], "1923-1928":["R", "Coolidge"], "1929-1932":["R", "Hoover"],
                    "1933-1944":["D", "F Roosevelt"], "1945-1952":["D", "Truman"], "1953-1960":["R", "Eisenhower"], "1961-1962":["D", "J Kennedy"],
                    "1963-1968":["D", "Johnson"], "1969-1973":["R", "Nixon"], "1974-1976":["R", "Ford"], "1977-1980":["D", "Carter"],
                    "1981-1988":["R", "Reagan"], "1989-1992":["R", "G Bush"], "1993-2000":["D", "Clinton"], "2001-2008":["R", "G W Bush"],
                    "2009-2016":["D", "Obama"], "2017-2020":["R", "Trump"], "2021-2021":["D", "Biden"]}
```
After this, imported our data using a csv file, then defined some neccesaary variables. We next looped through the data set, storing the starting and closing S&P 500 price for each president's term. After finding the closing price, we calculated the percent change for each presidents term.
```
# For each row in the datasheeet
for i in range(len(data["Unnamed: 0"])):

    # If the date is numeric (and not a label on the datasheet)
    if str(data["Unnamed: 0"][i])[1].isnumeric():

        # Assign the year and month to a variable
        year = int(str(data["Unnamed: 0"][i])[0:4])
        month = str(data["Unnamed: 0"][i])[5:]

        # If the year is the start of a new presidential term, save as the starting value
        if year == int(list(president_dict)[j][0:4]) and month == "01":
            starting_value = data["Unnamed: 1"][i]

        # If the year is the end of a presidential term, save as the ending value
        if year == int(list(president_dict)[j][5:]) and month == "12" or year == 2021 and month == "05":
            closing_value = data["Unnamed: 1"][i]

            # Calculate the percent change
            dif = ((float(closing_value) - float(starting_value)) / float(starting_value)) * 100

            j += 1
```
Following this, our two programs deviate slightly.

## by_presidents.py

In order to create a graph with a bar for each president, we added each president's percent change to a list. We then created a list of consiting of an "r" or "b" for each president in order to remember which color the bar should be (red for republican and blue for democrat).
```
# For each president in the dictionary
for key in president_dict.keys():

    # Add their name to the list
    names.append(president_dict[key][1])

    # Add a color based off of the political party
    if president_dict[key][0] == "R":
        colors.append("r")
    else:
        colors.append("b")
```

We then graphed the data and saved it. During the graphing process, we also wanted to label each bar with the percent change, so we used this code.
```
# Add the percent change at the end of each bar
for bar in bars:
    width = round(bar.get_width(), 2) #Previously we got the height
    label_y_pos = bar.get_y() + bar.get_height() / 2
    if width > 0:
        ax.text(width + 1, label_y_pos, s=f'{width}', va='center')
    else:
        ax.text(width - 13, label_y_pos, s=f'{width}', va='center')
```


## by_party.py

For this program, we don't want each individual president. Instead, we want the average of each party as a whole over the course of the entire time period. To do this, after finding the percent change, we used our dictionary fromt the beginning to add each percent change to the desired list, republican or democrat.
```
# Add to the desired list (either democrat or republican)
            if president_dict[list(president_dict)[j]][0] == "R":
                republican_list.append(dif)
            else:
                democrat_list.append(dif)
```
Next, we took the average of both lists. We then finished by labeling and coloring our bars, then graphing and saving our data.
  



