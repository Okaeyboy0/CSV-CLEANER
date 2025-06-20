import streamlit as st
import pandas as pd

st.set_page_config(page_title="CSV Cleaner")

st.title("🧹 CSV Cleaner Tool")

uploaded_file = st.file_uploader("📂 Upload your CSV file", type="csv")

if uploaded_file:
    df = pd.read_csv(uploaded_file)
    st.subheader("🔍 Original Data")
    st.dataframe(df)

    st.subheader("🧼 Cleaning Options")
    remove_empty_rows = st.checkbox("Remove empty rows")
    remove_empty_cols = st.checkbox("Remove empty columns")
    remove_duplicates = st.checkbox("Remove duplicate rows")

    if st.button("✨ Clean CSV"):
        cleaned_df = df.copy()

        if remove_empty_rows:
            cleaned_df = cleaned_df.dropna(how='all')

        if remove_empty_cols:
            cleaned_df = cleaned_df.dropna(axis=1, how='all')

        if remove_duplicates:
            cleaned_df = cleaned_df.drop_duplicates()

        st.success("✅ Cleaning done!")
        st.dataframe(cleaned_df)

        csv = cleaned_df.to_csv(index=False).encode('utf-8')
        st.download_button("⬇️ Download Cleaned CSV", csv, "cleaned_file.csv", "text/csv")
else:
    st.info("Please upload a CSV file to begin.")
