import javafx.application.Application; 
import javafx.geometry.Insets; import 
javafx.scene.Scene; import 
javafx.scene.control.*; import 
javafx.scene.layout.*; import 
javafx.stage.Stage;
import java.sql.*; import java.util.HashMap; import 
java.util.Map; public class ExpenseTrackerDBFX extends 
Application {
Stage window;
Scene mainScene, addScene, viewScene, summaryScene;
// JDBC variables
final String DB_URL = "jdbc:derby://localhost:1527/ExpenseDB"; 
final String USER = "app"; final String PASS = "app";
public static void main(String[] args) { launch(args);
}
// Expense class (for object reference)
class Expense {
String date; String 
category; double 
amount;
String description;
Expense(String date, String category, double amount, String description) { @Override
public void start(Stage primaryStage) {
window = primaryStage;
window.setTitle("Expense Tracker System (with JavaDB)");
// --- Main Menu Layout ---
VBox mainLayout = new VBox(15);
mainLayout.setPadding(new Insets(20));
Button btnAdd = new Button("Add Expense");
Button btnView = new Button("View Expenses");
Button btnSummary = new Button("View Summary"); Button 
btnExit = new Button("Exit");
mainLayout.getChildren().addAll(btnAdd, btnView, btnSummary, btnExit); 
mainScene = new Scene(mainLayout, 400, 300);
// --- Add Expense Scene ---
GridPane addLayout = new GridPane(); 
addLayout.setPadding(new Insets(20)); 
addLayout.setVgap(10); addLayout.setHgap(10);
Label dateLabel = new Label("Date (dd/mm/yyyy):"); TextField 
dateInput = new TextField();
Label categoryLabel = new Label("Category:");
ComboBox<String> categoryInput = new ComboBox<>();
categoryInput.getItems().addAll("Food", "Travel", "Shopping", "Bills", 
"Others");
Label amountLabel = new Label("Amount:"); TextField 
amountInput = new TextField();
Label descLabel = new Label("Description:"); TextField 
descInput = new TextField(); Button btnSubmit = new Button("Add"); Button 
btnBackAdd = new Button("Back");
addLayout.add(dateLabel, 0, 0); addLayout.add(dateInput, 
1, 0); addLayout.add(categoryLabel, 0, 1); 
addLayout.add(categoryInput, 1, 1); 
addLayout.add(amountLabel, 0, 2); 
addLayout.add(amountInput, 1, 2); 
addLayout.add(descLabel, 0, 3); addLayout.add(descInput, 
1, 3); addLayout.add(btnSubmit, 0, 4); 
addLayout.add(btnBackAdd, 1, 4); addScene = new 
Scene(addLayout, 450, 350);
// --- View Expenses Scene ---
VBox viewLayout = new VBox(10);
viewLayout.setPadding(new Insets(20));
Label viewTitle = new Label("All Recorded Expenses:"); TextArea 
expenseList = new TextArea(); expenseList.setEditable(false);
Button btnBackView = new Button("Back");
viewLayout.getChildren().addAll(viewTitle, expenseList, btnBackView); 
viewScene = new Scene(viewLayout, 450, 400);
// --- Summary Scene ---
VBox summaryLayout = new VBox(15);
summaryLayout.setPadding(new Insets(20));
Label summaryTitle = new Label("Expense Summary:"); TextArea 
summaryArea = new TextArea(); summaryArea.setEditable(false);
Button btnBackSummary = new Button("Back");
summaryLayout.getChildren().addAll(summaryTitle, summaryArea,
btnBackSummary); summaryScene = new 
Scene(summaryLayout, 450, 400);
// --- Button Actions ---
btnAdd.setOnAction(e -> window.setScene(addScene));
btnView.setOnAction(e -> { try (Connection con = DriverManager.getConnection(DB_URL, USER,
PASS);
Statement st = con.createStatement();
ResultSet rs = st.executeQuery("SELECT * FROM EXPENSES")) {
StringBuilder sb = new StringBuilder();
while (rs.next()) {
sb.append("Date: ").append(rs.getString("DATE"))
.append("\nCategory: ").append(rs.getString("CATEGORY")) 
.append("\nAmount: ₹").append(rs.getDouble("AMOUNT"))
.append("\nDescription:
").append(rs.getString("DESCRIPTION"))
.append("\n---------------------------------\n");
}
expenseList.setText(sb.length() > 0 ? sb.toString() : "No expenses 
recorded yet!"); window.setScene(viewScene);
} catch (Exception ex) { showAlert(Alert.AlertType.ERROR, "Error", 
"Database error: " + ex.getMessage());
}
});
btnSummary.setOnAction(e -> {
try (Connection con = DriverManager.getConnection(DB_URL, USER,
PASS);
Statement st = con.createStatement();
ResultSet rs = st.executeQuery("SELECT CATEGORY,
SUM(AMOUNT) AS TOTAL FROM EXPENSES GROUP BY CATEGORY")) {
double total = 0;
Map<String, Double> categoryTotals = new HashMap<>(); while 
(rs.next()) { categoryTotals.put(rs.getString("CATEGORY"),
rs.getDouble("TOTAL")); total += 
rs.getDouble("TOTAL");
}
StringBuilder sb = new StringBuilder("Total Expenses: ₹" + total + "\n\nCategory-wise Breakdown:\n"); for (String cat : 
categoryTotals.keySet()) { 
sb.append(cat).append(":
₹").append(categoryTotals.get(cat)).append("\n");
}
summaryArea.setText(sb.toString()); window.setScene(summaryScene);
} catch (Exception ex) { showAlert(Alert.AlertType.ERROR, "Error", 
"Database error: " + ex.getMessage());
}
}); btnExit.setOnAction(e -> 
window.close());
// --- Add Expense Logic --btnSubmit.setOnAction(e -> 
{
String date = dateInput.getText().trim();
String category = categoryInput.getValue();
String amountStr = amountInput.getText().trim(); String 
desc = descInput.getText().trim();
if (date.isEmpty() || category == null || amountStr.isEmpty() ||
desc.isEmpty()) { showAlert(Alert.AlertType.ERROR, "Error", "Please fill all 
fields!"); return; }
double amount; try 
{
amount = Double.parseDouble(amountStr);
if (amount <= 0) {
showAlert(Alert.AlertType.ERROR, "Error", "Amount must be
positive!"); return;
}
} catch (NumberFormatException ex) { 
showAlert(Alert.AlertType.ERROR, "Error", "Amount must be numeric!");
return; }
try (Connection con = DriverManager.getConnection(DB_URL, USER,
PASS); EXPENSES (DATE, CATEGORY, AMOUNT, DESCRIPTION) VALUES
(?, ?, ?, ?)")) {
ps.setString(1, date); ps.setString(2, 
category); ps.setDouble(3, amount); 
ps.setString(4, desc); 
ps.executeUpdate();
showAlert(Alert.AlertType.INFORMATION, "Success", "Expense 
Added Successfully!");
dateInput.clear(); 
categoryInput.setValue(null); 
amountInput.clear(); descInput.clear();
} catch (SQLException ex) {
showAlert(Alert.AlertType.ERROR, "Error", "Database insert failed: " + 
ex.getMessage());
}
});
btnBackAdd.setOnAction(e -> window.setScene(mainScene)); 
btnBackView.setOnAction(e -> window.setScene(mainScene)); 
btnBackSummary.setOnAction(e -> window.setScene(mainScene));
// --- Start Program --
window.setScene(mainScene); window.show();
}
private void showAlert(Alert.AlertType type, String title, String message) { 
Alert alert = new Alert(type);
alert.setTitle(title); alert.setHeaderText(null); 
alert.setContentText(message); 
alert.showAndWait();
}
}