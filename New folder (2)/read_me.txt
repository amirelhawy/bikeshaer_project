import numpy as np
import pandas as pd

CITY_DATA = { 'chicago': 'chicago.csv',
              'new york city': 'new_york_city.csv',
              'washington': 'washington.csv' }

def get_filters():
    
    """ set a funcation to filter and get the user specific input  in while loop """    
    
    print('Hello! Let\'s explore some US bikeshare data!')
    # get user input for city (chicago, new york city, washington). HINT: Use a while loop to handle invalid inputs
    while True:

        city = input('Enter the city name, Chicago, New York City or Washington: ').lower()
        if city not in CITY_DATA:
            print ('This is city is not valid!')
        else :
             break

    # get user input for month (all, january, february, ... , june)
    while True:
    

        month = input('Enter the month here: ').title()
        months = ['January', 'February', 'March', 'April', 'May', 'June']
        if month != 'all' and month not in months:
            print ('This is an invalid input pls ad a valid month!')
        else :
            break

    # get user input for day of week (all, monday, tuesday, ... sunday)
    while True:
        day = input('Enter the day here: ').title()
        days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
        if day not in days :
            print ('This is an invalid input pls ad a valid day!')

        else :
             break

    print('-'*40)
    
    return(city, month, day)



def load_data(city, month, day):
    
    """ loading the user choosin data into load funcation """
   

    # load data file into a dataframe
    df = pd.read_csv(CITY_DATA[city], parse_dates=['Start Time'])

    # extract month and day of week from Start Time to create new columns
    df ['Start Month'] = df['Start Time'].dt.month_name()
    df ['Start Day'] = df['Start Time'].dt.day_name()
    df ['Start Hour'] = df['Start Time'].dt.hour
    

    # filter by month if applicable
    if month != 'all':

        # filter by month to create the new dataframe
        df = df[df['Start Month'] == month]

    # filter by day of week if applicable
    if day != 'all':
        # filter by day of week to create the new dataframe
        df = df[df['Start Day'] == day]
    
        

    return df



def display_raw_data(df):
    """ display fist 5 raw if the user want and ask him if he want the next 5 raw or not  """
    i = 0
    raw = input("get the first 5 rows of data: yes /no").lower()
    pd.set_option('display.max_columns',200)

    while True:            
        if raw == 'no':
            break
        elif raw == 'yes':
            print(df[i:i+5]) 
            raw = input(" the next 5 rows of data: yes /no").lower()
            i += 5
        else:
            raw = input("\nYour input is invalid. Please enter only 'yes' or 'no'\n").lower()



    
def time_stats(df):
    

    """Displays statistics on the most frequent times of travel."""

    print('\nCalculating The Most Frequent Times of Travel...\n')
    

    # display the most common month
    common_month = df['Start Month'].mode()[0]
    print('Most common month:', common_month)


    # display the most common day 
    common_day_of_week = df['Start Day'].mode()[0]
    print('Most common day:', common_day_of_week)



    # display the most common start hour
    common_hour = df['Start Hour'].mode()[0]
    print('Most common start hour:',(common_hour))
    
    print('-'*40)       


def station_stats(df):
    """Displays statistics on the most popular stations and trip."""

    print('\nCalculating The Most Popular Stations and Trip...\n')
    # start_time = time.time()

    # display most commonly used start station
    commonly_used_start_station = df['Start Station'].mode()[0]
    print('Commonly used start station:', commonly_used_start_station)


    # display most commonly used end station
    commonly_used_end_station = df['End Station'].mode()[0]
    print('Commonly used end station:', commonly_used_end_station)



    # display most frequent combination of start station and end station trip
    common_trip = ('From ' + df['Start Station'] + ' to ' + df['End Station']).mode()[0]
    print('Common trip:', common_trip)


    # print("\nThis took %s seconds." % (time.time() - start_time))

    print('-'*40)

def trip_duration_stats(df):
    """Displays statistics on the total and average trip duration."""

    # display total travel time
    total_travel = df['Trip Duration'].sum()
    print ('Total travel time :' , total_travel , ' second ', ' or ', total_travel / 3600 , ' hour.')


    # display mean travel time
    avg_travel =df['Trip Duration'].mean()
    print ('Average travel time:', avg_travel, ' second ', ' or ', avg_travel / 3600 , ' hour.')

    print('-'*40)



def user_stats(df):
    """Displays statistics on bikeshare users."""
    
    try:
        
        # Display counts of user types
        counts_user_types = df['User Type'].value_counts()
        print (counts_user_types)


        # Display counts of gender
        counts_gender = df['Gender'].value_counts()
        print(counts_gender)

        # Display earliest, most recent, and most common year of birth
        earliest = df['Birth Year'].min()
        print('Earliest year of birth:', round(earliest))
        
        most_recent = df['Birth Year'].max()
        print('Most recent year of birth:', round(most_recent))
        
        most_common = df['Birth Year'].mode()[0]
        print('Most common year of birth :', round(most_common))
        
    except Exception:
        print('Some statistics are not available for Washington!')

    print('-'*40)


def main():
    while True:
        city, month, day = get_filters()
        df = load_data(city, month, day)

        display_raw_data(df)
        time_stats(df)
        station_stats(df)
        trip_duration_stats(df)
        user_stats(df)

        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower() != 'yes':
            break

if __name__ == "__main__":
    main()
