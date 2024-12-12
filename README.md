# perfadom
import streamlit as st
import pandas as pd

# Load data from Excel
file_path = 'Calculatrice_PERFADOM_VERS_FINALE1.xlsx'
calculatrice_data = pd.read_excel(file_path, sheet_name='Calculatrice PERFADOM')

# Streamlit app title
st.title("Calculette PERFADOM")

# Input form
def calculate_cost():
    with st.form(key='input_form'):
        st.header("Entrée des informations")

        # Indications dropdown
        indications = st.multiselect(
            "Indications",
            ["PERFUSION SA", "PERFUSION DIFFUSEURS", "PERFUSION GRAVITE", "NUTRITION", "NPAD"]
        )

        # Treatment details
        duree = st.number_input("Durée du traitement (jours)", min_value=1, step=1)
        perf_per_day = st.number_input("Nombre de perfusions par jour", min_value=1, step=1)

        # Perfadom selection
        perfadom_code = st.selectbox(
            "PERFADOM sélectionné",
            calculatrice_data['Code']
        )

        # Submit button
        submit_button = st.form_submit_button("Calculer")

        if submit_button:
            # Calculations
            total_perfusions = duree * perf_per_day
            tarif_unitaire = calculatrice_data.loc[
                calculatrice_data['Code'] == perfadom_code, 'Tarif TTC (€)'
            ].values[0]
            total_cost = total_perfusions * tarif_unitaire

            # Results display
            st.subheader("Résultats")
            st.write(f"Indications sélectionnées : {', '.join(indications)}")
            st.write(f"Durée du traitement : {duree} jours")
            st.write(f"Nombre total de perfusions : {total_perfusions}")
            st.write(f"Tarif unitaire : {tarif_unitaire} €")
            st.write(f"Coût total : {total_cost:.2f} €")

# Call the form and calculation function
calculate_cost()
