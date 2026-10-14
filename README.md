# Student-Performance-Analytics-System
import numpy as np
import pandas as pd

subjects = ["Maths", "Science", "English", "Computer", "Python"]


def get_grade(avg):
    if avg >= 90:
        return "A+"
    elif avg >= 80:
        return "A"
    elif avg >= 70:
        return "B"
    elif avg >= 60:
        return "C"
    elif avg >= 50:
        return "D"
    elif avg >= 40:
        return "E"
    return "F"
def get_result(row):
    if row[subjects].min() >= 40 and row["Attendance"] >= 75:
        return "Pass"
    return "Fail"

df = pd.read_csv("students.csv")
print(df.shape)
print(df.head())

marks = df[subjects].to_numpy()
df["Total"] = marks.sum(axis=1)
df["Average"] = np.round(marks.mean(axis=1), 2)
df["Grade"] = df["Average"].apply(get_grade)
df["Result"] = df.apply(get_result, axis=1)
print("\nStudent Results")
print(df[["Student_ID", "Name", "Total", "Average", "Grade", "Result"]].to_string(index=False))
print("\nClass Summary")
print("Class average:", round(df["Average"].mean(), 2))
print("Highest average:", df.loc[df["Average"].idxmax(), "Name"], df["Average"].max())
print("Lowest average:", df.loc[df["Average"].idxmin(), "Name"], df["Average"].min())
print("Passed:", (df["Result"] == "Pass").sum())
print("Failed:", (df["Result"] == "Fail").sum())
print("\nSubject-wise Performance")
for subject in subjects:
    print(subject, "-> avg:", round(df[subject].mean(), 2),
          "| highest:", df[subject].max(),
          "| lowest:", df[subject].min())

best = df[subjects].mean().idxmax()
print("Best subject:", best)

print("\nTop 5 Students")
print(df.sort_values("Average", ascending=False).head(5)[["Name", "Total", "Average", "Grade"]].to_string(index=False))
