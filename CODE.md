pip install openpyxl
import pandas as pd
from sklearn.cluster import KMeans
pip install matplotlib
import matplotlib.pyplot as plt
import numpy as np
file_path = "Untitled Folder/Dataset for vehicle routing and Inv managent.xlsx"  
sheet_data = pd.read_excel("Untitled Folder/Dataset for vehicle routing and Inv managent.xlsx", sheet_name="Sheet1")
locations = sheet_data[["Store_Location", "Demand"]]
locations["Location_Code"] = locations["Store_Location"].astype("category").cat.codes
locations_for_clustering = locations[["Location_Code", "Demand"]].to_numpy()
num_clusters = 4
kmeans = KMeans(n_clusters=num_clusters, random_state=0)
locations["Cluster"] = kmeans.fit_predict(locations_for_clustering)
inventory_data = sheet_data[["Date", "Product_ID", "Demand", "Stock_Level", "Lead_Time"]]
def restocking_suggestions(inventory):
    restock_plan = []
plt.figure(figsize=(8, 6))
for cluster in range(num_clusters):
    cluster_data = locations[locations["Cluster"] == cluster]
    plt.scatter(cluster_data["Location_Code"], cluster_data["Demand"], label=f"Cluster {cluster}")

plt.title("Store Location Clustering for Vehicle Routing")
plt.xlabel("Encoded Location")
plt.ylabel("Demand")
plt.legend()
plt.show()
def restocking_suggestions(inventory):
    restock_plan = []
    for _, row in inventory.iterrows():
        if row["Stock_Level"] < row["Demand"] * row["Lead_Time"]:
            restock_plan.append({
                "Date": row["Date"],
                "Product_ID": row["Product_ID"],
                "Suggested_Restock": (row["Demand"] * row["Lead_Time"]) - row["Stock_Level"]
            })
    return pd.DataFrame(restock_plan)

restock_plan = restocking_suggestions(inventory_data)

print("Restocking Suggestions:")
print(restock_plan.head())
