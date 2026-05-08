import java.sql.*;
import java.util.Scanner;

public class EmployeeManagement {

public static void main(String[] args) {

String url = "jdbc:mysql://localhost:3306/employeedb";
String user = "root";
String pass = "Sayali";

try 
{
Class.forName("com.mysql.cj.jdbc.Driver");

Connection con =
DriverManager.getConnection(url, user, pass);

Scanner sc = new Scanner(System.in);

System.out.println("Connected Successfully!\n");

while (true) 
{
System.out.println("===== EMPLOYEE MANAGEMENT =====");
System.out.println("1. Add Employee");
System.out.println("2. View Employees");
System.out.println("3. Update Employee");
System.out.println("4. Delete Employee");
System.out.println("5. Exit");
System.out.print("Choose: ");

int ch = sc.nextInt();

switch (ch) {

// ADD EMPLOYEE
case 1:
sc.nextLine();

System.out.print("Enter Employee ID: ");
int employeeID = sc.nextInt();
sc.nextLine();

System.out.print("Enter Name: ");
String employeeName = sc.nextLine();

System.out.print("Enter Salary: ");
double salary = sc.nextDouble();

PreparedStatement ps1 =
con.prepareStatement("INSERT INTO employee(EmployeeID, EmployeeName, salary) VALUES(?,?,?)");

ps1.setInt(1, employeeID);
ps1.setString(2, employeeName);
ps1.setDouble(3, salary);

ps1.executeUpdate();

System.out.println("Employee Added Successfully!\n");
break;

// VIEW EMPLOYEE
case 2:
Statement st = con.createStatement();

ResultSet rs =
st.executeQuery("SELECT * FROM employee");

System.out.println("\nEmployeeID | EmployeeName | SALARY");

while (rs.next()) {

System.out.println(
rs.getInt("EmployeeID") + " | " +
rs.getString("EmployeeName") + " | " +
rs.getDouble("salary"));
}
System.out.println();
break;

// UPDATE EMPLOYEE
case 3:
System.out.print("Enter Employee ID: ");
int uid = sc.nextInt();

System.out.print("Enter New Salary: ");
double sal = sc.nextDouble();

PreparedStatement ps2 =con.prepareStatement("UPDATE employee SET salary=? WHERE EmployeeID=?");
ps2.setDouble(1, sal);
ps2.setInt(2, uid);

int u = ps2.executeUpdate();

if (u > 0)
System.out.println("Employee Updated Successfully!\n");
else
System.out.println("Employee Not Found!\n");
break;

// DELETE EMPLOYEE
case 4:
System.out.print("Enter Employee ID: ");
int did = sc.nextInt();

PreparedStatement ps3 =con.prepareStatement("DELETE FROM employee WHERE EmployeeID=?");

ps3.setInt(1, did);

int d = ps3.executeUpdate();

if (d > 0)
System.out.println("Employee Deleted Successfully!\n");
else
System.out.println("Employee Not Found!\n");
break;

// EXIT
case 5:
System.out.println("Exiting Program...");
con.close();
sc.close();
return;

default:

System.out.println("Invalid Choice!\n");
}
}

} 
catch (Exception e) 
{
System.out.println("Error : " + e.getMessage());
}
}
}