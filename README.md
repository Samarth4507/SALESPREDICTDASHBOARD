
# **Sales Predicting Dashboard**

## **Project Overview**
This project is a **Sales Predicting Dashboard** built with **Streamlit**. It provides insights into sales trends, customer demographics, and product performance. Additionally, it includes a machine learning model to predict total sales based on units sold and price per unit.

### **Features**
- Interactive data filtering by product, country, gender, and date range.
- Data visualizations such as line charts, bar plots, and pie charts.
- Machine learning regression model for predicting total sales.
- Dark-themed user interface with custom styling for an enhanced user experience.

---

## **Setup and Execution**

### **Pre-requisites**
1. Python (>= 3.8)
2. Install the required libraries:
   ```bash
   pip install pandas streamlit plotly scikit-learn streamlit-option-menu matplotlib
   ```

### **Steps to Run**
1. Clone this repository:
   ```bash
   git clone https://github.com/Samarth4507/Sales-Predicting-dashboard.git
   cd Sales-Predicting-dashboard
   ```
2. Place the pre-trained regression model (`regression_model.pkl`) in the project directory. The model should predict `total_sales` using `units_sold` and `price_per_unit`.

3. Run the Streamlit application:
   ```bash
   streamlit run app.py
   ```
4. Open your browser and navigate to `http://localhost:8501`.

---

## **Dashboard Pages**

### **1. Home**
- Displays overall metrics like:
  - **Total Sales**: The sum of all sales in the filtered dataset.
  - **Units Sold**: The total number of units sold.
- Provides an overview of sales performance.

### **2. Sales**
- Line chart of sales trends by product over time.
- Useful for tracking product performance across different periods.

### **3. Trends**
- Monthly trends for:
  - **Total Sales**: Shows the growth or decline in sales over time.
  - **Units Sold**: Displays the quantity of products sold over time.

### **4. Product**
- Bar chart of total sales by product.
- Helps identify top-performing and low-performing products.

### **5. Gender & Country Sales**
- **Pie Chart**: Total sales distribution by gender.
- **Bar Chart**: Sales performance by country.
- Useful for demographic and regional analysis.

### **6. Prediction**
- Input **units sold** and **price per unit** to predict **total sales** using the regression model.
- Allows users to simulate sales outcomes for given inputs.

---

## **Description of the Machine Learning Model**

### **Model Overview**
- The regression model predicts **total sales** based on:
  - `units_sold`
  - `price_per_unit`

### **Training Process**
1. The dataset was preprocessed to handle missing values and remove outliers.
2. A simple linear regression model was trained using `Scikit-learn`.
3. Evaluation metrics:
   - **R² Score**
   - **Mean Absolute Error (MAE)**

### **Saving the Model**
- The trained model was saved using `pickle`:
   ```python
   import pickle
   with open('regression_model.pkl', 'wb') as file:
       pickle.dump(model, file)
   ```

### **Prediction Example**
- The model predicts total sales as follows:
   ```python
   input_data = [[10, 50.0]]  # Example: 10 units sold at $50/unit
   predicted_sales = model.predict(input_data)
   ```

---

## **Code Structure**
- **`app.py`**: Main application script containing the dashboard logic.
- **`regression_model.pkl`**: Pre-trained machine learning model for prediction.
- **Dataset**: Sales data loaded dynamically from a remote URL.

---

## **Inline Code Documentation**
The code includes detailed comments for ease of understanding. Examples:

### **Data Loading and Preprocessing**
```python
@st.cache_data
def get_data():
    url = "https://raw.githubusercontent.com/Samarth4507/Sales-Predicting-dashboard/main/sales_data.csv"
    try:
        # Load data from URL
        data = pd.read_csv(url)
        # Convert timestamp to datetime format
        data['timestamp'] = pd.to_datetime(data['timestamp'], format='%d-%m-%Y %H:%M', errors='coerce')
        # Drop rows with invalid timestamps
        data.dropna(subset=['timestamp'], inplace=True)
        return data
    except Exception as e:
        st.error(f"Error loading data from the URL: {e}")
        return pd.DataFrame()
```

### **Prediction Logic**
```python
if st.button("Predict Total Sales"):
    if units_sold > 0 and price_per_unit > 0:
        try:
            # Prepare input for prediction
            input_data = pd.DataFrame([[units_sold, price_per_unit]], columns=['units_sold', 'price_per_unit'])
            predicted_sales = model.predict(input_data)[0]
            st.success(f"Predicted Total Sales: ${predicted_sales:,.2f}")
        except Exception as e:
            st.error(f"Prediction failed: {e}")
```

