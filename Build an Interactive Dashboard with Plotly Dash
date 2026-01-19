# Import required libraries
import pandas as pd
import dash
from dash import html
from dash import dcc
from dash.dependencies import Input, Output
import plotly.express as px

# Read the airline data into pandas dataframe
spacex_df = pd.read_csv("spacex_launch_dash.csv")
max_payload = spacex_df['Payload Mass (kg)'].max()
min_payload = spacex_df['Payload Mass (kg)'].min()

# Create a dash application
app = dash.Dash(__name__)

# Create an app layout
app.layout = html.Div(children=[
    html.H1('SpaceX Launch Records Dashboard',
            style={'textAlign': 'center', 'color': '#503D36', 'font-size': 40}),
    
    dcc.Dropdown(id='site-dropdown',
                 options=[
                     {'label': 'All Sites', 'value': 'All Sites'},
                     {'label': 'CCAFS LC-40', 'value': 'CCAFS LC-40'},
                     {'label': 'CCAFS SLC-40', 'value': 'CCAFS SLC-40'},
                     {'label': 'KSC LC-39A', 'value': 'KSC LC-39A'},
                     {'label': 'VAFB SLC-4E', 'value': 'VAFB SLC-4E'}
                 ],
                 value='All Sites',
                 placeholder="Select a Launch Site Here",
                 searchable=True),
    html.Br(),
    html.Div(dcc.Graph(id='success-pie-chart')),
    html.Br(),

                                 html.P("Payload range (Kg):"),

    html.Div(dcc.Graph(id='success-payload-scatter-chart')),
])

# Callback for pie chart
@app.callback(
    Output('success-pie-chart', 'figure'),
    Input('site-dropdown', 'value')
)
def get_pie_chart(entered_site):
    if entered_site == 'All Sites':
        fig = px.pie(
            filtered_df,
            values='class',
            names='Launch Site',
            title='Total Successful Launches by Site'
        )
    else:
        filteredsite_df = spacex_df[spacex_df['Launch Site'] == entered_site].copy()
        filteredsite_df['Outcome'] = np.where(filteredsite_df['class'] == 1, 'Success', 'Failure')

        pie_data2 = filteredsite_df.groupby('Outcome')['class'].count().reset_index(name='Count')

        fig = px.pie(
            pie_data2,
            names='Outcome',
            values='Count',
            title=f'Success vs Failure for site {entered_site}'
        )

    return fig
# Run the app
if __name__ == '__main__':
    app.run()
