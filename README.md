# Booking Details Data Analysis

## Overview
This project involves a Python script that processes a dataset named `Booking_details.xlsx`, which contains booking information for services like facility rentals, birthday parties, and classes. The script loads the data, performs exploratory data analysis (EDA), cleans it by handling missing values, dropping unnecessary columns, and shortening IDs, then saves the cleaned data as a CSV file (`Booking_data.csv`). The dataset is reduced from 18 columns to 10 during the cleaning process, making it more focused and manageable for analysis. The script leverages key Python libraries for data manipulation and analysis, executed in a Google Colab environment.

---

## Columns
### Original Columns (18)
The initial dataset (`Booking_details.xlsx`) contains the following 18 columns:
1. **Booking ID**: Unique identifier for each booking (UUID, e.g., "279d92c6-ce26-47c0-8915-e45b77fe20e2").
2. **Customer ID**: Unique identifier for each customer (UUID, e.g., "00901ce3-3d86-4c97-bca2-40ccac2fb99f").
3. **Customer Name**: Customer's name (e.g., "Customer 1").
4. **Booking Type**: Type of booking (e.g., "Facility", "Birthday Party", "Class").
5. **Booking Date**: Date of the booking (e.g., "2025-05-30").
6. **Status**: Booking status (e.g., "Pending", "Confirmed").
7. **Class Type**: Type of class (e.g., "Art", "Dance"), relevant for "Class" bookings.
8. **Instructor**: Instructor name (e.g., "James Howard"), if applicable.
9. **Time Slot**: Booking time (e.g., "10:00:00" in 24-hour format).
10. **Duration (mins)**: Duration in minutes (e.g., 90, 120).
11. **Price**: Booking cost (e.g., 42.74).
12. **Facility**: Facility used (e.g., "Party Room", "Play Area").
13. **Theme**: Event theme (e.g., "Superhero"), mostly for birthday parties.
14. **Subscription Type**: Subscription details (all `NaN` in this dataset).
15. **Service Name**: Name of the service (e.g., "Party Room", "Art").
16. **Service Type**: Service category (e.g., "Facility", "Birthday Party").
17. **Customer Email**: Customer's email (e.g., "customer1@example.com").
18. **Customer Phone**: Customer's phone number (e.g., "001-730-9034").
# Perform Cleaning
## Important Libraries
The script relies on the following libraries:
1. **NumPy (`numpy` as `np`)**:
   - Used for numerical operations, though not heavily utilized in this script.
   - Version: Typically bundled with Pandas.
2. **Pandas (`pandas` as `pd`)**:
   - Core library for data manipulation (loading Excel, dropping columns, saving to CSV).
   - Key functions: `pd.read_excel()`, `df.drop()`, `df.to_csv()`.
3. **Seaborn (`seaborn` as `sns`)**:
   - Included for potential data visualization, but not used in the provided code.
   - Useful for future enhancements (e.g., plotting booking trends).
4. **Warnings**:
   - Used to suppress warning messages with `warnings.filterwarnings("ignore")`.
   - Ensures cleaner output in Colab.


## Data Load
```
df=pd.read_excel("Booking_details.xlsx")
```

## Shape
- **Original Shape**: (1000 rows, 18 columns)
  - Checked using `df.shape` after loading the Excel file.
- **Final Shape**: (1000 rows, 10 columns)
  - Reduced after dropping 8 columns during cleaning.

---

## Info
The `df.info()` method provides a summary of the dataset:
- **Original Dataset**:
  - 1000 entries (rows).
  - 18 columns with varying non-null counts (e.g., `Class Type` has 328 non-null, `Instructor` has fewer).
  - Data types: `object` (strings), `datetime64[ns]` (`Booking Date`), `float64` (`Duration (mins)`, `Price`).
- **Key Observations**:
  - Columns like `Subscription Type` are entirely null.
  - `Time Slot`, `Duration (mins)`, and `Facility` have missing values (`NaN`).

---

## Missing Values
The dataset contains missing values (`NaN`) in several columns:
- **Class Type**: Missing for non-class bookings (672 missing out of 1000).
- **Instructor**: Missing unless a class has an assigned instructor (most are `NaN`).
- **Time Slot**: Missing for some bookings (e.g., birthday parties without a fixed slot).
- **Duration (mins)**: Missing in some rows, later filled with defaults like 84 minutes in the final output.
- **Facility**: Missing for class bookings, replaced with "No Facility" in the cleaned data.
- **Theme**: Missing unless a birthday party specifies one.
- **Subscription Type**: Completely missing (1000 `NaN` values).
- **Customer Phone**: Missing in some rows (e.g., Customer 997).

### Handling Missing Values
- **Duration (mins)**: Some `NaN` values are replaced with defaults (e.g., 84 minutes), though not explicitly coded.
- **Facility**: `NaN` values for classes are set to "No Facility".
- **Other Columns**: Dropped columns (e.g., `Theme`, `Instructor`) eliminate the need to handle their missing values.

---

## Dropped Columns
The script drops 8 columns to simplify the dataset:
- **Columns Dropped**: 
  - `Class Type`
  - `Instructor`
  - `Theme`
  - `Subscription Type`
  - `Service Name`
  - `Service Type`
  - `Customer Email`
  - `Customer Phone`
- **Code**:
  ```python
  df = df.drop(columns=["Class Type", "Instructor", "Theme", "Subscription Type", 
                        "Service Name", "Service Type", "Customer Email", "Customer Phone"])
  ```
- **Reason**: These columns were either redundant (e.g., `Service Type` overlaps with `Booking Type`), entirely null (`Subscription Type`), or not critical for core analysis (e.g., `Customer Email`, `Customer Phone`).

---
## Changed dtype and values round figure
```
df['Duration (mins)'] = df['Duration (mins)'].round(0).astype(int)
df['Price'] = df['Price'].round(0).astype(int)
```

## Renamed Columns
The script does not explicitly rename columns in the provided code. However, for clarity in the final output:
- **Duration (mins)** appears as `Duration(mins)` (without a space) in some outputs, suggesting a minor renaming or formatting adjustment might have occurred outside the shown code.
- No other renaming is evident, but you could add this step if desired (e.g., `df.rename(columns={"Duration (mins)": "Duration_mins"})`).

---
## Shorten IDs: 
- The Booking ID and Customer ID columns, originally lengthy UUIDs, are shortened to their first four characters (e.g., "279d92c6-..." to "279d", "00901ce3-..." to "0090") using string slicing. This simplifies the identifiers for easier reference and display while maintaining uniqueness within the dataset.
```
df["Booking ID"] = df["Booking ID"].str[:4]
df["Customer ID"] = df["Customer ID"].str[:4]
```
---


## Conversion to CSV
The cleaned DataFrame is exported to a CSV file:
- **File Name**: `Booking_data.csv`
- **Code**:
  ```python
  df.to_csv("Booking_data.csv", index=False)
  ```
- **Details**:
  - `index=False` prevents writing row indices to the CSV.
  - The resulting file contains 1000 rows and 10 columns, with cleaned and simplified data.
- **Purpose**: CSV format is lightweight, widely compatible, and suitable for further analysis or sharing.

---
### Final Columns (10)
After cleaning, the dataset (`Booking_data.csv`) retains these 10 columns:
1. **Booking ID**: Truncated to 4 characters (e.g., "279d").
2. **Customer ID**: Truncated to 4 characters (e.g., "0090").
3. **Customer Name**: Unchanged (e.g., "Customer 1").
4. **Booking Type**: Unchanged (e.g., "Facility").
5. **Booking Date**: Unchanged (e.g., "2025-05-30").
6. **Status**: Unchanged (e.g., "Pending").
7. **Time Slot**: Converted to 12-hour format (e.g., "10:00 AM").
8. **Duration (mins)**: Adjusted with defaults for missing values (e.g., 84).
9. **Price**: Rounded to integers (e.g., 43).
10. **Facility**: Updated with "No Facility" for classes (e.g., "Party Room").

---
## Sample Final Data
| Booking ID | Customer ID | Customer Name | Booking Type   | Booking Date | Status   | Time Slot | Duration (mins) | Price | Facility    |
|------------|-------------|---------------|----------------|--------------|----------|-----------|-----------------|-------|-------------|
| 279d       | 0090        | Customer 1    | Facility       | 2025-05-30   | Pending  | 10:00 AM  | 90              | 43    | Party Room  |
| 415b       | b82d        | Customer 2    | Birthday Party | 2025-05-29   | Pending  | 02:00 PM  | 84              | 182   | Party Room  |
| 2100       | 6bbb        | Customer 3    | Birthday Party | 2025-05-09   | Confirmed| 11:00 AM  | 120             | 208   | Play Area   |

---
# Dashboard Creation in Power BI
After cleaning, the dataset with 10 columns (Booking ID, Customer ID, Customer Name, Booking Type, Booking Date, Status, Time Slot, Duration (mins), Price, Facility) is used to create an interactive dashboard in Power BI. This dashboard visualizes booking trends, status distribution, and pricing insights, enabling efficient analysis and decision-making based on the streamlined data.

---

## 📄 Report Structure

### Page 1 - **Overview Dashboard**

- **11 Visual Elements**
- Includes:
  - **Cards**: Displaying KPIs such as Total Revenue, Total Bookings, and Average Booking Value.
  - **Stacked Chart**: Visualizing revenue by booking types.
  - **Line Chart**: Booking trend analysis over time.
  - **Pie Chart**: count booking over facility.
  - **Slicer**: Filters for Customer Name, Destination, and Payment Status.

### Page 2 - **Detailed Analysis**

- **6 Visual Elements**
- Includes:
  - **Ribbon chart**
  - **Waterfall Chart**
  - **Stacked Coluumn Chart**
  - **Stacked bar Chart**

---


# Preview



![1](https://github.com/user-attachments/assets/e6d98da2-4e26-4a85-9186-24652058a0f9)





![2](https://github.com/user-attachments/assets/cff43eaf-faf3-4ccc-9b68-b2474c263f47)







https://github.com/user-attachments/assets/368db8c1-684f-43eb-aaaf-e7eeff8025c9



📊 Key Insights:
 1). Total Revenue:
  - Total revenue generated from all bookings = 139k
   
 2). Booking Status:
  - Number of confirmed vs pending bookings → 490:510

 3). Most Popular Booking Type:
  - Most popular booking type = Facility

  Facility → 337 bookings

  Birthday Party → 335 bookings

  Class → 328 bookings


4). Booking Status:
 - Total Confirmed Bookings = 490
 - Total Pending Bookings = 510


5). Most Popular Time Slot:
 - Most popular time slot = 02:00 PM


6). Most Used Facility:
  - Most booked facility = Play Area

  Play Area → 356 times

  No Facility → 328 times

  Party Room → 316 times

7). Average Duration of Booking:
  - Average booking duration= 84.33 minutes

8). Monthly Booking Trend:
  -  Monthly distribution of bookings:

   March → 84 bookings

   April → 334 bookings

   May → 334 bookings

  June → 248 bookings

 - The highest number of bookings happened in April, which is tied with May .




## 🔄 Navigation & Interactivity

- The report includes **Page Navigation Buttons** for a smoother user experience.

---





