# AJP
Practical:-1


Aim:- Implement TCP Server for transferring files using Socket and ServerSocket.
Code:-
Server.java
import java.io.*; import java.net.*; class Server
{
public static void main(String args[]) throws Exception
{
try (ServerSocket ss = new ServerSocket(4000); Socket s = ss.accept()) { System.out.println("connected.	");
FileInputStream fin=new FileInputStream("Server.txt"); DataOutputStream dout=new DataOutputStream(s.getOutputStream()); int r;
while((r=fin.read())!=-1)
{
dout.write(r);
}
System.out.println("\n File tranfer Completed");
}
}
}

Client.java
import java.io.*; import java.net.*;

public class Client {
public static void main(String[] args) throws Exception
{
try (Socket s = new Socket("127.0.0.1",4000)) { if(s.isConnected())
{
System.out.println("Connected to server");
}
FileOutputStream fout;
fout = new FileOutputStream("Client.txt");
DataInputStream din=new DataInputStream(s.getInputStream()); int r;
while((r=din.read())!=-1)
{
fout.write((char)r);
}
}
}
}
 



Output:-


Server.java
 


Client.java
 


Server.txt

 


Client.txt

 
 
Practical:-2


Aim:- Implement cookies to store firstname and lastname using Java server pages.
Code:-
Index.html
<!DOCTYPE html>
<html>
<head>
<title>Setting Cookies</title>
</head>
<body>
<h1>Setting Cookies</h1>
<form action="main.jsp" method="GET">
First Name: <input type="text" name="first_name"> <br /><br/> Last
Name: <input type="text" name="last_name" /><br/><br/> <input type="submit" value="Submit" />
</form>
</body>
</html>


Main.jsp
<%
// Create cookies for first and last names.
Cookie firstName = new Cookie("first_name", request.getParameter("first_name")); Cookie lastName = new Cookie("last_name", request.getParameter("last_name"));

// Set expiry date after one hour for both the cookies. firstName.setMaxAge(60 * 60);
lastName.setMaxAge(60 * 60);

// Add both the cookies in the response header. response.addCookie(firstName); response.addCookie(lastName);
%>
<html>
<body>
<h1>Setting Cookies</h1>
<ul>
<b>First Name:</b><%=request.getParameter("first_name")%><br /><br/>
<b>Last Name:</b><%=request.getParameter("last_name")%>
</body>
</html>
 
Output:-


 



  
Practical:-3


Aim:- Implement the shopping cart for users for the online shopping. Apply the concept of session.
Code:-
Web.xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="4.0" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web- app_4_0.xsd">
<servlet>
<servlet-name>main</servlet-name>
<servlet-class>main</servlet-class>
</servlet>
<servlet-mapping>
<servlet-name>main</servlet-name>
<url-pattern>/main</url-pattern>
</servlet-mapping>
<session-config>
<session-timeout> 30
</session-timeout>
</session-config>
</web-app>

Main.java (Servlet)
import java.util.ArrayList; import java.util.List;

import javax.servlet.ServletException; import javax.servlet.annotation.WebServlet; import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest; import javax.servlet.http.HttpServletResponse; import javax.servlet.http.HttpSession;

@WebServlet("/main")
public class main extends HttpServlet {
private static final long serialVersionUID = 1L;

protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException {
HttpSession session = request.getSession(true);
List<String> cart = (List<String>) session.getAttribute("cart");

if (cart == null) {
cart = new ArrayList<>(); session.setAttribute("cart", cart);
}

// Display the cart items response.setContentType("text/html");
 
try {
response.getWriter().println("<html><body>"); response.getWriter().println("<h2>Shopping Cart</h2>");

if (cart.isEmpty()) {
response.getWriter().println("<p>Your cart is empty.</p>");
} else {
response.getWriter().println("<ul>"); for (String item : cart) {
response.getWriter().println("<li>" + item + "</li>");
}
response.getWriter().println("</ul>");
}

// Add product form response.getWriter().println("<h3>Add Product</h3>"); response.getWriter().println("<form method='post'>");
response.getWriter().println("Product name: <input type='text' name='product' required><br>");
response.getWriter().println("<input type='submit' value='Add to Cart'>"); response.getWriter().println("</form>");

response.getWriter().println("</body></html>");
} catch (Exception e) { e.printStackTrace();
}
}

protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException {
HttpSession session = request.getSession(true);
List<String> cart = (List<String>) session.getAttribute("cart");

if (cart == null) {
cart = new ArrayList<>(); session.setAttribute("cart", cart);
}

// Retrieve product from the form
String product = request.getParameter("product");

// Add the product to the cart cart.add(product);

// Redirect back to the cart page try {
response.sendRedirect(request.getContextPath() + "/cart");
} catch (Exception e) { e.printStackTrace();
}
}

protected void doDelete(HttpServletRequest request, HttpServletResponse response) throws ServletException {
HttpSession session = request.getSession(true);
List<String> cart = (List<String>) session.getAttribute("cart");
 

if (cart != null) {
String product = request.getParameter("remove"); cart.remove(product);
}

// Redirect back to the cart page try {
response.sendRedirect(request.getContextPath() + "/cart");
} catch (Exception e) { e.printStackTrace();
}
}
}


Output:-



 
Practical:- 4



Aim:- Implement student registration form with enrollment number, first name, last name, semester, contact number. Store the details in database. Also implement search, delete and modify facility for student records.
Code:-
Index.html
<html>
<head>
<title>Employee Details</title>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body>

<form method="POST" action="employee">

<table width="400px" border="1">
<tr>
<td colspan="2"><h1>Student Regestration</h1> </td>

</tr>

<tr>
<td>Enrollment</td>
<td><input type="text" name="empid" id="empid"></td>
</tr>

<tr>
<td>Firstname</td>
<td><input type="text" name="fname" id="fname"></td>
</tr>

<tr>
<td>Lastname</td>
<td><input type="text" name="lname" id="lname"></td>
</tr>
<tr>
<td>Semester</td>
<td><input type="text" name="num" id="num"></td>
</tr>
<tr>
<td>Number</td>
<td><input type="text" name="sem" id="sem"></td>
</tr>

<tr>
<td colspan="2"> <input type="submit" value="submit"></td>

</tr>
 
</table>

</form>
<p><a href="viewemployee">View Employee</a></p>

</body>
</html>

Delete.java (Servlet)
import java.io.IOException; import java.io.PrintWriter; import java.sql.Connection; import java.sql.DriverManager;
import java.sql.PreparedStatement; import java.sql.ResultSet;
import java.sql.SQLException; import java.util.logging.Level; import java.util.logging.Logger;
import javax.servlet.ServletException; import javax.servlet.annotation.WebServlet; import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest; import javax.servlet.http.HttpServletResponse;

@WebServlet("/Delete")
public class Delete extends HttpServlet {

Connection con; PreparedStatement pst; ResultSet rs;
int row;


public void doGet(HttpServletRequest req,HttpServletResponse rsp ) throws IOException,ServletException
{

rsp.setContentType("text/html"); PrintWriter out = rsp.getWriter();

String stuenrollment = req.getParameter("enrollment");


try {
Class.forName("com.mysql.jdbc.Driver");
con = DriverManager.getConnection("jdbc:mysql://localhost/record","root","root"); pst = con.prepareStatement("delete from data where enrollment = ?"); pst.setString(1, stuenrollment);
row = pst.executeUpdate();

out.println("<font color='green'> Record Deleted </font>");


} catch (ClassNotFoundException ex) { Logger.getLogger(students.class.getName()).log(Level.SEVERE, null, ex);
 
} catch (SQLException ex) {

out.println("<font color='red'> Record Failed </font>");

}


}

}

EditServlet.java (Servlet)
import java.io.IOException; import java.io.PrintWriter; import java.sql.Connection; import java.sql.DriverManager;
import java.sql.PreparedStatement; import java.sql.ResultSet;
import java.sql.SQLException; import java.util.logging.Level; import java.util.logging.Logger;
import javax.servlet.ServletException; import javax.servlet.annotation.WebServlet; import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest; import javax.servlet.http.HttpServletResponse;

@WebServlet("/EditServlet")
public class EditServlet extends HttpServlet {


Connection con; PreparedStatement pst; ResultSet rs;
int row;

public void doPost(HttpServletRequest req,HttpServletResponse rsp ) throws IOException,ServletException
{

rsp.setContentType("text/html"); PrintWriter out = rsp.getWriter();

try {
Class.forName("com.mysql.jdbc.Driver");
con = DriverManager.getConnection("jdbc:mysql://localhost/lba","root","root");

String empid = req.getParameter("empid"); String fname = req.getParameter("fname"); String lname = req.getParameter("lname"); String sem = req.getParameter("sem"); String num = req.getParameter("num");

pst = con.prepareStatement("update employee set fname = ?, lname = ? where id = ?"); pst.setString(1, fname);
pst.setString(2, lname);
 
pst.setString(3, empid); pst.setString(1, sem); pst.setString(1, num);
row = pst.executeUpdate();

out.println("<font color='green'> Record Updateeeedd </font>");

} catch (ClassNotFoundException ex) { Logger.getLogger(employee.class.getName()).log(Level.SEVERE, null, ex);
} catch (SQLException ex) {

out.println("<font color='red'> Record Failed </font>");

}

}

}

Editreturn.java (Servlet)
import java.io.IOException; import java.io.PrintWriter; import java.sql.Connection; import java.sql.DriverManager;
import java.sql.PreparedStatement; import java.sql.ResultSet;
import java.sql.SQLException; import java.sql.Statement; import java.util.logging.Level; import java.util.logging.Logger;
import javax.servlet.ServletException; import javax.servlet.annotation.WebServlet; import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest; import javax.servlet.http.HttpServletResponse;

@WebServlet("/Editreturn")
public class Editreturn extends HttpServlet {

Connection con; PreparedStatement pst; ResultSet rs;
int row;

public void doGet(HttpServletRequest req,HttpServletResponse rsp ) throws IOException,ServletException
{

rsp.setContentType("text/html"); PrintWriter out = rsp.getWriter();
String eenrollment = req.getParameter("enrollment"); try {
Class.forName("com.mysql.jdbc.Driver");
 
con = DriverManager.getConnection("jdbc:mysql://localhost/record","root","root");

pst = con.prepareStatement("select * from data where enrollment = ?"); pst.setString(1, eenrollment);
rs = pst.executeQuery();

while(rs.next())
{
out.print("<form action='EditServlet' method='POST'"); out.print("<table");

out.print("<tr> <td>Enrollment</td>	<td> <input type='text' name ='enrollment' id='enrollment' value= '" + rs.getString("enrollment") + "'/> </td> </tr>");
out.print("<tr> <td>Firstname</td>	<td> <input type='text' name ='fname' id='fname' value= '" + rs.getString("fname") + "'/> </td> </tr>");
out.print("<tr> <td>Lastname</td>	<td> <input type='text' name ='lname' id='lname' value= '" + rs.getString("lname") + "'/> </td> </tr>");
out.print("<tr> <td>Semester</td>	<td> <input type='text' name ='sem' id='sem' value= '" + rs.getString("sem") + "'/> </td> </tr>");
out.print("<tr> <td>Contact</td>	<td> <input type='text' name ='snum' id='snum' value= '" + rs.getString("snum") + "'/> </td> </tr>");
out.print("<tr> <td colspan ='2'> <input type='submit' value= 'Edit'/> </td> </tr>"); out.print("</table");
out.print("</form");

}

} catch (ClassNotFoundException ex) { Logger.getLogger(students.class.getName()).log(Level.SEVERE, null, ex);
} catch (SQLException ex) {

out.println("<font color='red'> Record Failed </font>");
}
}

}

Students.java (Servlet)
import java.io.IOException; import java.io.PrintWriter; import java.sql.Connection; import java.sql.DriverManager;
import java.sql.PreparedStatement; import java.sql.SQLException; import java.util.logging.Level; import java.util.logging.Logger;
import javax.servlet.ServletException; import javax.servlet.annotation.WebServlet; import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest; import javax.servlet.http.HttpServletResponse;


@WebServlet("/students")

public class students extends HttpServlet {
 

Connection con; PreparedStatement pst; int row;

@Override
public void doPost(HttpServletRequest req,HttpServletResponse rsp ) throws IOException,ServletException
{

rsp.setContentType("text/html"); PrintWriter out = rsp.getWriter();


try {
Class.forName("com.mysql.jdbc.Driver");
con = DriverManager.getConnection("jdbc:mysql://localhost/record","root","root"); String stuenrollment = req.getParameter("enrollment");
String stufname = req.getParameter("fname"); String stulname= req.getParameter("lname"); String stusem= req.getParameter("sem"); String stusnum= req.getParameter("snum");

pst = con.prepareStatement("insert into data(enrollment,fname,lname,sem,snum)values(?,?,?,?,?) ");
pst.setString(1, stuenrollment); pst.setString(2, stufname); pst.setString(3, stulname); pst.setString(4, stusem); pst.setString(5, stusnum);
row = pst.executeUpdate();

out.println("<font color='green'> Record Added </font>");

} catch (ClassNotFoundException ex) { Logger.getLogger(students.class.getName()).log(Level.SEVERE, null, ex);
} catch (SQLException ex) {

out.println("<font color='red'> Record Failed </font>");
}
}

}

Viewstudents.java (Servlet)
import java.io.IOException; import java.io.PrintWriter; import java.sql.Connection; import java.sql.DriverManager;
import java.sql.PreparedStatement; import java.sql.ResultSet;
import java.sql.SQLException; import java.sql.Statement; import java.util.logging.Level; import java.util.logging.Logger;
 
import javax.servlet.ServletException; import javax.servlet.annotation.WebServlet; import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest; import javax.servlet.http.HttpServletResponse;

@WebServlet("/viewstudents")
public class viewstudents extends HttpServlet {

Connection con; PreparedStatement pst; ResultSet rs;
int row;

public void doGet(HttpServletRequest req,HttpServletResponse rsp ) throws IOException,ServletException
{

rsp.setContentType("text/html"); PrintWriter out = rsp.getWriter();


try {
Class.forName("com.mysql.jdbc.Driver");
con = DriverManager.getConnection("jdbc:mysql://localhost/record","root","root"); String sql;
sql = "select * from data";
Statement stmt = con.createStatement(); rs = stmt.executeQuery(sql);

out.println("<table cellspacing='0' width='350px' border='1'>"); out.println("<tr>");
out.println("<td> Enrollment </td>"); out.println("<td> Firstname </td>"); out.println("<td> Lastname </td>"); out.println("<td> Sem </td>"); out.println("<td> Contact </td>"); out.println("<td> Edit </td>"); out.println("<td> Delete </td>");
out.println("</tr>"); while(rs.next())
{
out.println("<tr>");
out.println("<td>" + rs.getString("enrollment") + "</td>"); out.println("<td>" + rs.getString("fname") + "</td>"); out.println("<td>" + rs.getString("lname") + "</td>"); out.println("<td>" + rs.getString("sem") + "</td>"); out.println("<td>" + rs.getString("snum") + "</td>");

out.println("<td>" + "<a href='Editreturn?enrollment=" + rs.getString("enrollment") + "'> Edit </a>" + "</td>");
 
out.println("<td>" + "<a href='Delete?enrollment=" + rs.getString("enrollment") + "'> Delete </a>" + "</td>");
out.println("</tr>");

}

out.println("</table>");

} catch (ClassNotFoundException ex) { Logger.getLogger(students.class.getName()).log(Level.SEVERE, null, ex);
} catch (SQLException ex) {

out.println("<font color='red'> Record Failed </font>");
}
}
}



Output:-


Form



Record Added

 
View Students




Edit Students



Delete Students

 
Practical:-5


Aim:- Write a Servlet program to print system date and time.
Code:-
Server.java
import java.io.*; import javax.servlet.*;
public class DateSrv extends GenericServlet
{
public void service(ServletRequest req, ServletResponse res) throws IOException, ServletException
{
//set response content type res.setContentType("text/html");
//get stream obj
PrintWriter pw = res.getWriter();
//write req processing logic
java.util.Date date = new java.util.Date();
pw.println("<h2>"+"Current Date & Time: " +date.toString()+"</h2>");
//close stream object pw.close();
}
}


Output:-



 
Practical:-6



Aim:- Design a web page that takes the Username from user and if it is a valid username prints “Welcome Username”. Use JSF to implement.
Code:-
Index.html
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
<form action="loginPage" method="Post">
User Name:<input type="text" name="uname"/><br/><br> Password:<input type="password" name="upass"/><br/><br>
<input type="submit" value="SUBMIT"/>
</form>
</body>
</html>

Login.java
import java.io.IOException; import java.io.PrintWriter;
import javax.servlet.RequestDispatcher; import javax.servlet.ServletException; import javax.servlet.annotation.WebServlet; import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest; import javax.servlet.http.HttpServletResponse; @WebServlet("/loginPage")
public class Login extends HttpServlet { private static final long serialVersionUID = 1L; public Login() {
super();
}
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
//response.getWriter().append("Served at: ").append(request.getContextPath());
}
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
doGet(request, response); response.setContentType("text/html"); PrintWriter pwriter = response.getWriter(); String name=request.getParameter("uname"); String pass=request.getParameter("upass"); if(name.equals("Admin") && pass.equals("root"))
{
PrintWriter pw = response.getWriter();
 
pw.print("Hello "+name+"!<br>"); pw.print(" Welcome to this site!");
}
else
{
pwriter.print("Username or password is incorrect!"); RequestDispatcher dis=request.getRequestDispatcher("index.html"); dis.include(request, response);
}
}
}

Output:-
Invalid Login



Valid Login

