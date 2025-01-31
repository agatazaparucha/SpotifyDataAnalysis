
# Spotify-Dashboard

### Dashboard Link : https://app.powerbi.com/links/0RRNeJt3hJ?ctid=73689ee1-b42f-4e25-a5f6-66d1f29bc092&pbi_source=linkShare

## Problem Statement

This dashboard offers a deeper and more comprehensive analysis of a user’s listening habits compared to the official Spotify Wrapped. It analyzes data spanning the user’s entire listening history (in this case, 2019–2024).

The first dashboard focuses on artists and albums. It provides visualizations of:

- Total minutes spent listening to top artists,
- "Truly liked" albums (albums with the highest number of songs played at least 10 times),
- Listening statistics broken down by country,
- Most liked songs,
- The average skip rate across different devices.

The second dashboard highlights time-based trends. It includes visualizations such as:
- Average listening time per day,
- The average number of songs played per hour,
- A heatmap showing the days with the most music played,
- A bar chart illustrating the division between day and night listening habits.
A slicer is also available, allowing users to explore their data for specific years, months, or even individual days.


### Steps followed 

- Step 1 : Load data into Power BI Desktop: multiple JSON files containing data from different years.
- Step 2 : Merge data into 1 file: make the cleaning proccess easier and avoid mistakes.
- Step 3 : In Power Query. Replace missing data accordingly, get rid of columns unnecessary for the analysis. Divide the date and time column.
- Step 4 : Create a time and calendar table, connect it with our main data table.
- Step 5 : Think of possible questions the data might answer - choose correct visualizations and create necessary measures for them.

Here are some DAX expressions, including the most interesting measures used in the analysis.
        
        Favorite Song per Album = 
        VAR MaxPlays = 
            CALCULATE(
            MAXX(
                FILTER(
                    'Streaming_History_2020-2024',
                    'Streaming_History_2020-2024'[album_name] = SELECTEDVALUE('Streaming_History_2020-2024'[album_name])
                ),
                [Song Play Count]
            )
        )
        RETURN
            CALCULATE(
                FIRSTNONBLANK('Streaming_History_2020-2024'[track_name], 1),
                    FILTER(
                'Streaming_History_2020-2024',
                'Streaming_History_2020-2024'[album_name] = SELECTEDVALUE('Streaming_History_2020-2024'[album_name]) &&
                [Song Play Count] = MaxPlays
            )
        )

or:

    Top Artist by Country = 
    VAR MaxArtistTime = 
        CALCULATE(
            MAXX(
                VALUES('Streaming_History_2020-2024'[artist_name]),
                [Total Listening Time by Artist]
            ),
            ALLEXCEPT('Streaming_History_2020-2024', 'Streaming_History_2020-2024'[NewCountry])
        )

    RETURN
        CALCULATE(
            FIRSTNONBLANK('Streaming_History_2020-2024'[artist_name], 1),
            FILTER(
                VALUES('Streaming_History_2020-2024'[artist_name]),
            [Total Listening Time by Artist] = MaxArtistTime
            )
        )
        

- Step 6 : Add the visuals to the dashboard. Visual filters (slicers) were added for the date hierarchy. The viewer can now drill down from year to a singular day.
- Step 7 : Using visual level filter from the filters pane, basic filtering was used & unknown artist values were unselected for some of the visuals.
           
 - Step 8 : A color scheme was chosen and applied to all visuals. The charts have been cleaned up. Correct data formats were chosen, the titles of the visuals fixed

 - Step 9 : The report was then published to Power BI Service.
 

# Report Snapshot (Power BI DESKTOP)

![Image](https://github.com/user-attachments/assets/0fe7e236-488f-47f8-8caa-91412b9ec4b6)

![Image](https://github.com/user-attachments/assets/9dd7e405-cb8e-4dbc-9425-f2c03da7b7ad)

# Insights

This two-page report was created using Power BI Desktop and later published to Power BI Service.

In the future, leveraging my expanding knowledge of data pipelines, I aim to automate this process. This would allow friends and family to easily upload their data and receive personalized insights into their own listening habits.


