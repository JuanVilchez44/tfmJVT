# tfmJVT
import pandas as pd
ruta_archivo = '/Users/juanvilchez/Desktop/dfjugadores.xlsx'
df = pd.read_excel(ruta_archivo)
# Convertir las columnas relevantes a numérico, manejando errores
df['stats_Starts'] = pd.to_numeric(df['stats_Starts'], errors='coerce')
df['passing_Cmp%'] = df['passing_Cmp%'].astype(str).str.replace('%', '').astype(float)
df['defense_Tkl+Int'] = pd.to_numeric(df['defense_Tkl+Int'], errors='coerce')
df['passing_PrgP'] = pd.to_numeric(df['passing_PrgP'], errors='coerce')
df['misc_Recov'] = pd.to_numeric(df['misc_Recov'], errors='coerce')
df['misc_Won%'] = df['misc_Won%'].astype(str).str.replace('%', '').astype(float)
df_filtered = df[df['stats_Starts'] > 17]
df_filtered = df_filtered[df_filtered['stats_Pos'].str.contains('MF')]
weights = {
    'passing_Cmp%': 0.25,
    'defense_Tkl+Int': 0.15,
    'passing_PrgP': 0.30,
    'misc_Recov': 0.20,
    'misc_Won%': 0.10,
}
df_filtered['weighted_score'] = (
    df_filtered['passing_Cmp%'] * weights['passing_Cmp%'] +
    df_filtered['defense_Tkl+Int'] * weights['defense_Tkl+Int'] +
    df_filtered['passing_PrgP'] * weights['passing_PrgP'] +
    df_filtered['misc_Recov'] * weights['misc_Recov'] +
    df_filtered['misc_Won%'] * weights['misc_Won%']
)
top_defensive_midfielders = df_filtered[['Player', 'stats_Pos', 'stats_Squad', 'weighted_score','passing_Cmp%','defense_Tkl+Int','passing_PrgP','misc_Recov','misc_Won%','stats_Gls','stats_Ast']].sort_values(by='weighted_score', ascending=False).head(60)
from IPython.display import display
display(top_defensive_midfielders)
jugadores_seleccionados = ['Bruno Guimarães','Youssouf Fofana','Joshua Kimmich','Kirian Rodríguez','Maxence Caqueret']
selected_players = top_defensive_midfielders[top_defensive_midfielders['Player'].isin(jugadores_seleccionados)]
from IPython.display import display
display(selected_players)
