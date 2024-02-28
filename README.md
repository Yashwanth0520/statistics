import pandas as ps
import plotly.graph_objs as go
from plotly.offline import iplot


def load_data(filename):
    """
    Load data from a CSV file into a Pandas DataFrame.

    Parameters:
    - filename (str): The path to the CSV file containing the data.

    Returns:
    - DataFrame: A Pandas DataFrame containing the loaded data.
    """
    return pd.read_csv(filename)


def display_info(df, filename):
    """
    Display information about the DataFrame.

    Parameters:
    - df (DataFrame): The DataFrame to display information about.
    - filename (str): The name of the file loaded.
    """
    print(f"Data loaded from file: {filename}")
    print(df.info())
    print(df.head(10))


def visualize_data(df):
    """
    Visualize university rankings data.

    Parameters:
    - df (DataFrame): The DataFrame containing the university rankings data.
    """
    df_2015 = df[df.year == 2015]

    trace0 = go.Box(
        y=df_2015.total_score,
        name='Total score of universities in 2015',
        marker=dict(
            color='rgb(12, 12, 140)',
        )
    )
    trace1 = go.Box(
        y=df_2015.research,
        name='Research score of universities in 2015',
        marker=dict(
            color='rgb(12, 128, 128)',
        )
    )
    data = [trace0, trace1]
    iplot(data)

    subset_2014 = df[df.year == 2014].iloc[:3, :]

    trace1 = go.Bar(
        x=subset_2014.university_name,
        y=subset_2014.citations,
        name="Citations",
        marker=dict(color='rgba(255, 174, 255, 0.5)',
                    line=dict(color='rgb(0,0,0)', width=1.5)),
        text=subset_2014.country)

    trace2 = go.Bar(
        x=subset_2014.university_name,
        y=subset_2014.teaching,
        name="Teaching",
        marker=dict(color='rgba(255, 255, 128, 0.5)',
                    line=dict(color='rgb(0,0,0)', width=1.5)),
        text=subset_2014.country)
    data = [trace1, trace2]
    layout = go.Layout(barmode="group")
    fig = go.Figure(data=data, layout=layout)
    iplot(fig)

    subset_2014 = df[df.year == 2014].iloc[:100, :]
    subset_2015 = df[df.year == 2015].iloc[:100, :]
    subset_2016 = df[df.year == 2016].iloc[:100, :]

    trace1 = go.Scatter(
        x=subset_2014.world_rank,
        y=subset_2014.citations,
        mode="markers",
        name="2014",
        marker=dict(color='rgba(255, 128, 255, 0.8)'),
        text=subset_2014.university_name)

    trace2 = go.Scatter(
        x=subset_2015.world_rank,
        y=subset_2015.citations,
        mode="markers",
        name="2015",
        marker=dict(color='rgba(255, 128, 2, 0.8)'),
        text=subset_2015.university_name)

    trace3 = go.Scatter(
        x=subset_2016.world_rank,
        y=subset_2016.citations,
        mode="markers",
        name="2016",
        marker=dict(color='rgba(0, 255, 200, 0.8)'),
        text=subset_2016.university_name)
    data = [trace1, trace2, trace3]
    layout = dict(
        title='Citation vs world rank of top 100 universities with 2014, 2015 and 2016 years',
        xaxis=dict(title='World Rank', ticklen=5, zeroline=False),
        yaxis=dict(title='Citation', ticklen=5, zeroline=False)
    )
    fig = dict(data=data, layout=layout)
    iplot(fig)


# Load data
filename = "timesData.csv"
df = load_data(filename)

# Display information about the data
display_info(df, filename)

# Visualize the data
visualize_data(df)
