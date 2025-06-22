{
 "cells": [
  {
   "cell_type": "markdown",
   "id": "1403a83f",
   "metadata": {},
   "source": [
    "# GHG Emissions Preprocessing Project\n",
    "This notebook loads and preprocesses GHG emission data by commodity from a 2016 summary dataset."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "93b50a0f",
   "metadata": {},
   "outputs": [],
   "source": [
    "import pandas as pd"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "817b46e7",
   "metadata": {},
   "outputs": [],
   "source": [
    "# Load the Excel file\n",
    "file_path = \"SupplyChainEmissionFactorsforUSIndustriesCommodities (4).xlsx\"\n",
    "sheet_name = \"2016_Summary_Commodity\"\n",
    "\n",
    "# Read the Excel sheet into DataFrame\n",
    "df = pd.read_excel(file_path, sheet_name=sheet_name)\n",
    "df.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "6654fea5",
   "metadata": {},
   "outputs": [],
   "source": [
    "# Drop irrelevant and DQ columns\n",
    "df_clean = df.drop(columns=[col for col in df.columns if \"Unnamed\" in col or \"DQ\" in col])\n",
    "\n",
    "# Rename important columns\n",
    "df_clean = df_clean.rename(columns={\n",
    "    \"Commodity Code\": \"Code\",\n",
    "    \"Commodity Name\": \"Commodity\",\n",
    "    \"Substance\": \"GHG\",\n",
    "    \"Supply Chain Emission Factors with Margins\": \"EmissionFactor\"\n",
    "})\n",
    "\n",
    "# Filter to key GHGs\n",
    "ghg_focus = [\"carbon dioxide\", \"methane\", \"nitrous oxide\"]\n",
    "df_filtered = df_clean[df_clean[\"GHG\"].isin(ghg_focus)].copy()\n",
    "\n",
    "# Drop missing emission factor rows\n",
    "df_filtered.dropna(subset=[\"EmissionFactor\"], inplace=True)\n",
    "\n",
    "df_filtered.head()"
   ]
  }
 ],
 "metadata": {},
 "nbformat": 4,
 "nbformat_minor": 5
}
