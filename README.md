package EBMS;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.Random;
import java.util.Scanner;

public class Bill {
	String cn = "";
	private static final Scanner Sc = new Scanner(System.in);


	public void admin() {
		while (true) {
			System.out.println(
					"------------------------Welcome to Electricity Bill Management System-------------------------------------");
			System.out.println(" ");
			System.out.println(" 1 FOR ADMIN LOGIN ");
			System.out.println(" 2 FOR USER LOGIN ");
			System.out.println(" 0 FOR EXIT ");

			Scanner sc = new Scanner(System.in);
			System.out.print("Select the option: ");
			int select = sc.nextInt();
			if (select == 1) {
				System.out.println("\nAdmin Login Page");
				System.out.print("Enter the User Name : ");
				String username = sc.next();
				String Uname = "Gauhar@gmail.com";

				System.out.print("Enter the password: ");
			    String password = sc.next();
				String Upassword = "Gauhar@123";
				if (username.equals(Uname) && password.equals(Upassword)) {
					System.out.println("Admin Login Successfully");

					admin_Dashboard();

				} else {
					System.out.println("Error! Invalid User Name and Password");
				}
			} else if (select == 2) {

				System.out.println("\nUser Login Page");
				System.out.print("Enter the User Name : ");
				String username = sc.next();
				String Uname = "Gauhar@gmail.com";

				System.out.print("Enter the password: ");
				String password = sc.next();
				String Upassword = "Gauhar2004";
				if (username.equals(Uname) && password.equals(Upassword)) {
					System.out.println("User Login Successfully");

					user_Dashboard();

				}

				else {
					System.out.println("Error! Invalid User Name and Password ");
				}

			} else if(select==0) {
				System.out.println("Thanks for visiting !!");
				break;

			}
			else {
				System.out.println("wrong option");

			}

		}
	}
	
	

//	public void CreateDB() {
//		try {
//			String url = "jdbc:Mysql://localhost:3306";
//			String user = "root";
//			String pass = "shams817";
//			Connection con = DriverManager.getConnection(url, user, pass);
//			Statement smt = con.createStatement();
//			String q = "Create Database BMS";
//			smt.execute(q);
//			System.out.println("Database Create");
//		} catch (Exception e) {
//			System.out.println("Connection Error " + e);
//		}
//
//	}

//-----------------------REGISTER_NEW_CUSTOMER_TABLE-------------------------------------------------------		

	public void Register_new_customer() {
		try {
			String url = "jdbc:Mysql://localhost:3306/BMS";
			String user = "root";
			String pass = "shams817";
			Connection con = DriverManager.getConnection(url, user, pass);
			Statement smt = con.createStatement();
			String q = "Create table Register_new_customer(Bill_ID varchar(30) primary key , Full_Name varchar(40), Father_Name varchar(40), Aadhar_Number varchar(40), Contact_Number varchar(20), E_mail varchar(30), Address varchar(50), State varchar(20), Pin_code varchar (20), Consumer_Number varchar(50))";
			smt.execute(q);
			System.out.println("Customer Table created");
		} catch (Exception e) {
			System.out.println("Connection Error " + e);
		}
	}
	
	
	public void user_Dashboard() {
		while(true) {
			System.out.println("\n----------------WELCOME TO USER DASHBOARD-------------");
			int select;
			Scanner sc = new Scanner(System.in);
			System.out.println(" 1.View Profile");
			System.out.println(" 2.View Bill");
			System.out.println(" 3.View Bill Status");
			System.out.println(" 4.Pay Bill");
			System.out.println(" 5.Request Upgrade KW");
			System.out.println(" 6.Register Complain");
			System.out.println(" 7.View Complain Status");

			System.out.print("Select the option: ");
			select = sc.nextInt();
			if (select == 1) {
				profile();
				user_Dashboard();
			} else if (select == 2) {
				System.out.println("View Bill");
				View_Bill();
				user_Dashboard();
			} else if (select == 3) {
				System.out.println("View_Bill_Status");
				View_Bill();
				user_Dashboard();
			} else if (select == 4) {
				System.out.println("Pay_Bill");
				Pay_Bill();
				user_Dashboard();
			} else if (select == 5) {
				System.out.println("Request_Upgrade KW");
				Upgrade_KW();
				user_Dashboard();
			} else if (select == 6) {
				System.out.println("Register_Complain");
				Complain_table_data();
				user_Dashboard();
			} else if (select == 7) {
				
				System.out.println("\nView Complain Status");
				complain_status();

			} else if (select == 0) {
				admin();
			} else {
				admin();
			}
		}

		}

public void Pay_Bill() {
	Scanner sc = new Scanner(System.in);
		try {
			double bill_amount=0;
			String bill_status = "";
			
			String url = "jdbc:mysql://localhost:3306/bms";
			String user = "root";
			String pass = "shams817";
			
			
			Connection conn = DriverManager.getConnection(url, user, pass);
			
			String q = "SELECT Bill_Id, Month_Name, pay_Amount, Status FROM generate_bill WHERE Bill_Id=?";
            PreparedStatement pst = conn.prepareStatement(q);
            BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
            System.out.print("Enter your Bill_Id: ");
            String Bill_Id = br.readLine();
			pst.setString(1, Bill_Id);
            ResultSet rs = pst.executeQuery();
            
            while (rs.next()) {
            	System.out.println("\nBill Detail");
            	System.out.println("--------------");
            	System.out.println("Bill_Id: " + rs.getString("Bill_Id"));
                System.out.println("Month: " + rs.getString("Month_Name"));
                System.out.println("Amount: Rs. " + rs.getDouble("Pay_Amount"));
                System.out.println("Status: " + rs.getString("Status"));  
                System.out.println("-------------------------------------");
                bill_amount = rs.getDouble("Pay_Amount");
                bill_status = rs.getString("Status");
            }
            
            String bill_paid = "paid";
            
            if(bill_paid.equals(bill_status)) {
            	System.out.println("No Dues!");
            	
            } else {
            	
            	String payType = "";      
                while (true) {
                	System.out.println("Select Payment Mode");
                    System.out.println("1. UPI");
                    System.out.println("2. Debit/Credit Card");
                    System.out.println("3. Net Banking");
                    System.out.print("Enter choice (1-3): ");
                    int choice = sc.nextInt();
                    sc.nextLine();
                    
                    String upi_id = "shams@oksbi";
                    int cardNo = 1234;
                    int cvv = 817;
                    long net_banking_no = 930441393;
                    
                    if (choice == 1) {
                    	
                        payType = "UPI";
                        System.out.print("Enter your UPI_ID : ");
                        String upi = sc.next();
                        if(upi.equals(upi_id)) {
                        	System.out.print("Enter Payable Amount Rs: ");
                        	double amt = sc.nextDouble();
                        	
                        	if(amt == bill_amount) {
                        		
                        		
                        		String query = "UPDATE generate_bill SET status = 'paid' WHERE Bill_Id = ?";
                            	PreparedStatement psmt = conn.prepareStatement(query);
                            	
                            	psmt.setString(1, Bill_Id);
                            	psmt.executeUpdate();
                            	System.out.println("Payment Successfully!");
                            	break;
                        	} else {
                        		System.out.println("Invalid Amount / Payment Failed!");
                        	}
                        } else {
                        	System.out.println("Invalid UPI Id / Payment Failed!");
                        }
                        
                        break;
                    } else if (choice == 2) {
                    	
                    	payType = "Debit/Credit Card";
                        
                        System.out.print("Enter Last 4 digit of your Card: ");
                        int cardNum = sc.nextInt();
                        System.out.print("Enter CVV Number: ");
                        int cvvNum = sc.nextInt();
                        
                        if(cardNum == cardNo && cvvNum == cvv) {
                        	System.out.print("Enter Payable Amount Rs: ");
                        	double amt = sc.nextDouble();
                        	
                        	//double bill_amount;
                        	
    						if(amt == bill_amount) {
                        		
                        		
                        		String query = "UPDATE generate_bill SET status = 'paid' WHERE Bill_Id = ?";
                            	PreparedStatement psmt = conn.prepareStatement(query);
                            	
                    			psmt.setString(1, Bill_Id);
                            	psmt.executeUpdate();
                            	System.out.println("Payment Successfully");
                            	break;
                        	}
    						else {
    							System.out.println("Invalid Amount / Payment fail");
    						}
                        	
                        	}
                        else {
                        	System.out.println("Invalid ATM No / Payment fail");
                        }
                        break;
                    } else if(choice == 3) {
                    	
                    	payType = "Net Banking";
                    	System.out.print("Enter you Net Banking Mobile Number: ");
                    	long net = sc.nextLong();
                    	
                    	if(net == net_banking_no) {
                    		System.out.print("Enter Payable Amount Rs: ");
                        	double amt = sc.nextDouble();
                        	
                        	if(amt == bill_amount) {
                        		
                        		String query = "UPDATE generate_bill SET status = 'paid' WHERE Bill_Id = ?";
                            	PreparedStatement psmt = conn.prepareStatement(query);
                    			psmt.setString(1, Bill_Id);
                            	psmt.executeUpdate();
                            	System.out.println("Payment Successfully");
                        	}
    						else {
    							System.out.println("Invalid Amount / Payment Failed!");
    						}
                        	
                    	} else {
                    		System.out.println("Invalid Number / Payment Failed!");
                    	}
                    	
                    } else {
                        System.out.println("Invalid choice! Please select 1, 2 or 3.");
                    }
                }
            	
            }
            
			conn.close();
			
		} catch(Exception e) {
			
			System.out.println("Error" + e);
		}
	}
		
	

//---------------------INSERT CUSTOMER------------------------------------------------------

	public void Insert_customer() {
		try {

			String url = "jdbc:mysql://localhost:3306/BMS";

			String user = "root";

			String pass = "shams817";

			Connection con = DriverManager.getConnection(url, user, pass);

			String q = "insert into Register_new_customer(Full_Name, Father_Name, Aadhar_Number, Contact_Number, E_mail, Address, State, Pin_code, Consumer_Number, KW)values(?,?,?,?,?,?,?,?,?,?)";

			PreparedStatement ps = con.prepareStatement(q);

			BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

			System.out.println("Enter the Full_Name");

			String Full_Name = br.readLine();

			ps.setString(1, Full_Name);

			System.out.println("Enter the Father_Name");

			String Father_Name = br.readLine();

			ps.setString(2, Father_Name);

			System.out.println("Enter the Aadhar_Number");

			String Aadhar_number = br.readLine();

			ps.setString(3, Aadhar_number);

			System.out.println("Enter the Contact_Number");

			String Contact_Number = br.readLine();

			ps.setString(4, Contact_Number);

			System.out.println("Enter the E_mail");

			String E_mail = br.readLine();

			ps.setString(5, E_mail);

			System.out.println("Enter the Address");

			String Address = br.readLine();

			ps.setString(6, Address);

			System.out.println("Enter the State");

			String State = br.readLine();

			ps.setString(7, State);

			System.out.println("Enter the Pin_code");

			String Pin_code = br.readLine();

			ps.setString(8, Pin_code);

			System.out.println("Enter the Consumer_Number");

			String Consumer_Number = br.readLine();

			ps.setString(9, Consumer_Number);
			
			System.out.println("Enter the Demand KW");

			String KW = br.readLine();

			ps.setString(10, KW);
			// ..................................Captcha
			// Code.................................................

			int random = 0, i, cc, sum = 0;
			Scanner sc = new Scanner(System.in);
			Random rand = new Random();
			System.out.print("Captcha Code: ");
			for (i = 0; i < 3; i++) {
				random = rand.nextInt(10);
				System.out.print(random);
				if (i < 2) {
					System.out.print(" + ");
				}

				sum = sum + random;
			}
			System.out.println("");
			System.out.print("Enter the Captcha Code (Sum of the Number) = ");
			cc = sc.nextInt();
			if (sum == cc) {
				ps.executeUpdate();
				System.out.println("Captcha Code Matched");
				System.out.println("Data Inserted");
			} else {
				System.out.println("Captcha Code Not Matched");
				System.out.println("No! Data Inserted");
			}

			con.close();

		} catch (Exception e)

		{
			System.out.println("Connection Error" + e);
		}
	}

	// --------------------CUSTOMER VIEW------------------------

	public void View_Customer() {
		try {
			String url = "jdbc:Mysql://localhost:3306/BMS";
			String user = "root";
			String pass = "shams817";
			Connection con = DriverManager.getConnection(url, user, pass);

			String q = "select * from Register_new_customer where Consumer_Number=?";
			PreparedStatement ps = con.prepareStatement(q);

			BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

			System.out.println("Enter Consumer_Number : ");
			String Consumer_Number = br.readLine();
			ps.setString(1, Consumer_Number);

			ResultSet rs = ps.executeQuery();
			boolean hasResults=false;
			while (rs.next()) {
				hasResults=true;
				System.out.println("Full_Name : " + rs.getString(1));
				System.out.println("Father_Name : " + rs.getString(2));
				System.out.println("Aadhar_number :" + rs.getString(3));
				System.out.println("Contact_Number :" + rs.getString(4));
				System.out.println("E_mail : " + rs.getString(5));
				System.out.println("Address : " + rs.getString(6));
				System.out.println("State : " + rs.getString(7));
				System.out.println("Pin_code : " + rs.getString(8));
				System.out.println("Consumer_Number : " + rs.getString(9));
				System.out.println(" ");
			}
			if (!hasResults) {
		        System.out.println("Warning: No data found in the result set.");
		    }
			
			
			con.close();
		} catch (Exception e) {
			System.out.println("Database access Error " + e);
		}

	}

	public void profile() {
		try {
			String url = "jdbc:Mysql://localhost:3306/BMS";
			String user = "root";
			String pass = "shams817";
			Connection con = DriverManager.getConnection(url, user, pass);

			String q = "select * from Register_new_customer where Consumer_Number=?";
			PreparedStatement ps = con.prepareStatement(q);

			BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
			System.out.println("\nView Profile");
			System.out.println("Enter Consumer_Number : ");
			String Consumer_Number = br.readLine();
			ps.setString(1, Consumer_Number);

			ResultSet rs = ps.executeQuery();
			while (rs.next()) {
				System.out.println("Full_Name : " + rs.getString(1));
				System.out.println("Father_Name : " + rs.getString(2));
				System.out.println("Aadhar_number :" + rs.getString(3));
				System.out.println("Contact_Number :" + rs.getString(4));
				System.out.println("E_mail : " + rs.getString(5));
				System.out.println("Address : " + rs.getString(6));
				System.out.println("State : " + rs.getString(7));
				System.out.println("Pin_code : " + rs.getString(8));
				System.out.println("Consumer_Number : " + rs.getString(9));
				System.out.println(" ");
			}
			con.close();
		} catch (Exception e) {
			System.out.println("Database access Error " + e);
		}

	}

	// -------------------------BILL_TABLE------------------------------------

	public void Generate_bill() {
		try {
			String url = "jdbc:Mysql://localhost:3306/BMS";
			String user = "root";
			String pass = "shams817";
			Connection con = DriverManager.getConnection(url, user, pass);
			Statement smt = con.createStatement();
			String q = "Create table Generate_bill(Bill_Id varchar(30) primary key , Current_Unit varchar(50), Previous_Unit varchar(50), Month_Name varchar(20), Area varchar(50), KW varchar(50), Perunit_charge varchar(50), Subsidy varchar(50), Pay_Amount varchar(50), Consumer_Number varchar(50))";
			smt.execute(q);
			System.out.println("Generate_bill Table created");
		} catch (Exception e) {
			System.out.println("Connection Error " + e);
		}
	}

//---------------BILLING_DATA-----------------------------

	public void genrate_bill_data() {
		try {
			String url = "jdbc:Mysql://localhost:3306/BMS";
			String user = "root";
			String pass = "shams817";
			Connection con = DriverManager.getConnection(url, user, pass);
			Statement smt = con.createStatement();
			String q1 = "select * from Register_new_customer where Consumer_Number=?";
			PreparedStatement ps1 = con.prepareStatement(q1);
			BufferedReader br1 = new BufferedReader(new InputStreamReader(System.in));

			System.out.println("Enter Consumer_Number : ");
			String Consumer_Number = br1.readLine();
			ps1.setString(1, Consumer_Number);

			ResultSet rs = ps1.executeQuery();
			while (rs.next()) {

				System.out.println("Full_Name : " + rs.getString(1));
				System.out.println("Father_Name : " + rs.getString(2));
				System.out.println("Contact_Number :" + rs.getString(4));
				System.out.println("Address : " + rs.getString(6));
				System.out.println("Consumer_Number : " + rs.getString(9));
				cn = rs.getString(9);
				System.out.println(" ");
			}

			String q = "insert into Generate_bill(Bill_Id, Current_Unit, Previous_Unit, Month_Name, Area , KW , Perunit_charge, Subsidy, Pay_Amount,Consumer_Number, Status)Values (?,?,?,?,?,?,?,?,?,?,?)";
			PreparedStatement ps = con.prepareStatement(q);
			BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

			System.out.println("Enter the Bill_Id: ");
			String Bill_Id = br.readLine();
			ps.setString(1, Bill_Id);

			System.out.println("Enter the Current_Unit");
			int Current_Unit = Integer.parseInt(br.readLine());
			ps.setInt(2, Current_Unit);

			System.out.println("Enter the Previous_Unit");
			int Previous_Unit = Integer.parseInt(br.readLine());
			ps.setInt(3, Previous_Unit);

			System.out.println("Enter the Month_Name");
			String Month_Name = br.readLine();
			ps.setString(4, Month_Name);

			System.out.println("Enter the Area");
			String Area = br.readLine();
			ps.setString(5, Area);

			System.out.println("Enter the KW");
			String KW = br.readLine();
			ps.setString(6, KW);

			System.out.println("Enter the Perunit_charge");
			int Perunit_charge = Integer.parseInt(br.readLine());
			ps.setInt(7, Perunit_charge);

			int this_month_Unit =  Current_Unit - Previous_Unit;
			int amount = this_month_Unit * Perunit_charge;
			int Subsidy;
			int Pay_Amount;

			if (amount >= 5000) {
				Subsidy = (amount * 5) / 100;
				Pay_Amount = amount - Subsidy;
				ps.setInt(8, Subsidy);
				ps.setInt(9, Pay_Amount);

			} else {
				Pay_Amount = amount;
				Subsidy = 0;
				ps.setInt(8, Subsidy);
				ps.setInt(9, Pay_Amount);
			}

			ps.setString(10, cn);
			ps.setString(11, "Unpaid");

//------------------------------CAPTCHA_CODE----------------------------------

			int random = 0, i, cc, sum = 0;
			Scanner sc = new Scanner(System.in);
			Random rand = new Random();
			System.out.print("Captcha Code: ");
			for (i = 0; i < 3; i++) {
				random = rand.nextInt(10);
				System.out.print(random);
				if (i < 2) {
					System.out.print(" + ");
				}

				sum = sum + random;
			}
			System.out.println("");
			System.out.print("Enter the Captcha Code (Sum of the Number) = ");
			cc = sc.nextInt();
			if (sum == cc) {
				ps.executeUpdate();
				System.out.println("Captcha Code Matched");
				System.out.println("Bill Generated!!");
			} else {
				System.out.println("Captcha Code Not Matched");
				System.out.println("No! Data Inserted");
			}

			con.close();

		} catch (Exception e)

		{
			System.out.println("Connection Error" + e);
		}
	}

//-----------------------VIEW BILL----------------------

	public void View_Bill() {

		try {
			String url = "jdbc:Mysql://localhost:3306/BMS";
			String user = "root";
			String pass = "shams817";
			Connection con = DriverManager.getConnection(url, user, pass);

			String q = "select * from Generate_bill where Consumer_Number=?";
			PreparedStatement ps = con.prepareStatement(q);

			BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

			System.out.print("Enter Consumer_Number : ");
			String Consumer_Number = br.readLine();
			ps.setString(1, Consumer_Number);

			ResultSet rs = ps.executeQuery();
			boolean hasResults=false;
			while (rs.next()) {
				hasResults=true;
				System.out.println("Bill_Id : " + rs.getString(1));
				System.out.println("Current_Unit : " + rs.getString(2));
				System.out.println("Previous_Unit : " + rs.getString(3));
				System.out.println("Month_Name :" + rs.getString(4));
				System.out.println("Area :" + rs.getString(5));
				System.out.println("KW: " + rs.getString(6));
				System.out.println("Perunit_charge : " + rs.getString(7));
				System.out.println("Subsidy : " + rs.getString(8));
				System.out.println("Pay_Amount  : " + rs.getString(9));
				System.out.println("Consumer_Number : " + rs.getString(10));
				System.out.println("Status: " + rs.getString(11));
			}
			if (!hasResults) {
		        System.out.println("Warning: No data found in the result set.");
		        
		    }
			
			con.close();
		} catch (Exception e) {
			System.out.println("Database access Error " + e);
		}

	}

//--------------------COMPLAIN_TABLE-----------------------------------

	public void Complain_table() {

		try {
			String url = "jdbc:Mysql://localhost:3306/BMS";
			String user = "root";
			String pass = "shams817";
			Connection con = DriverManager.getConnection(url, user, pass);
			Statement smt = con.createStatement();
			String q = "Create table Complain_table (Consumer_Number varchar(50), Consumer_Name varchar(50),Phone_Number varchar(50),Msg varchar(5000),Status varchar(50))";
			smt.execute(q);
			System.out.println("Complain_Table created");
		} catch (Exception e) {
			System.out.println("Table Error " + e);
		}
	}

	// -------------------COMPLAIN_DATA----------------------------------

	public void Complain_table_data() {

		try {
			String url = "jdbc:Mysql://localhost:3306/BMS";
			String user = "root";
			String pass = "shams817";
			Connection con = DriverManager.getConnection(url, user, pass);
			Statement smt = con.createStatement();
			String q = "insert into Complain_table (Consumer_Number, Consumer_Name, Phone_Number, Msg, Status) values(?,?,?,?,?)";
			PreparedStatement ps = con.prepareStatement(q);
			BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
			System.out.println("Enter the Consumer_Number");
			String Consumer_Number = br.readLine();
			ps.setString(1, Consumer_Number);
			System.out.println("Enter the Consumer_Name");
			String Consumer_Name = br.readLine();
			ps.setString(2, Consumer_Name);
			System.out.println("Enter the phone_Number");
			String Phone_Number = br.readLine();
			ps.setString(3, Phone_Number);
			System.out.println("Enter the MSG");
			String Msg = br.readLine();
			ps.setString(4, Msg);
//			System.out.println("Enter the Status");
//			String Status=br.readLine();

			ps.setString(5, "progress");
			ps.executeUpdate();
			con.close();
			System.out.println("Complain Regisert SUCCESSFULY ");
		} catch (Exception e) {
			System.out.println("Table Error " + e);
		}
	}

//-----------------------VIEW_COMPLAIN-------------------------------

	public void View_Complain() {

		try {
			String url = "jdbc:Mysql://localhost:3306/BMS";
			String user = "root";
			String pass = "shams817";
			Connection con = DriverManager.getConnection(url, user, pass);

			String q = "select * from Complain_table where Consumer_Number=?";
			PreparedStatement ps = con.prepareStatement(q);

			BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

			System.out.println("Enter Consumer_Number : ");
			String Consumer_Number = br.readLine();
			ps.setString(1, Consumer_Number);

			ResultSet rs = ps.executeQuery();
			while (rs.next()) {
				System.out.println("Consumer_Number : " + rs.getString(1));
				System.out.println("Consumer_Name : " + rs.getString(2));
				System.out.println("Phone_Number : " + rs.getString(3));
				System.out.println("Msg :" + rs.getString(4));

				System.out.println(" ");
			}
			System.out.println("Get Data");
			con.close();
		} catch (Exception e) {
			System.out.println("Database access Error " + e);
		}

//-------------------VENDOR_TABLE------------------------		

	}
	
	public void complain_status() {

		try {
			String url = "jdbc:Mysql://localhost:3306/BMS";
			String user = "root";
			String pass = "shams817";
			Connection con = DriverManager.getConnection(url, user, pass);

			String q = "select * from complain_table where Consumer_Number=?";
			PreparedStatement ps = con.prepareStatement(q);

			BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

			System.out.print("Enter Consumer_Number : ");
			String Consumer_Number = br.readLine();
			ps.setString(1, Consumer_Number);

			ResultSet rs = ps.executeQuery();
			
			while (rs.next()) {
				System.out.println("Consumer_Number : " + rs.getString(1));
				System.out.println("Consumer_Name : " + rs.getString(2));
				System.out.println("Phone_Number : " + rs.getString(3));
				System.out.println("Message : " + rs.getString(4));
				System.out.println("Status: " + rs.getString(5));
			}
			
			con.close();
		} catch (Exception e) {
			System.out.println("Database access Error " + e);
		}

	}

	public void Vendor_Id() {
		try {
			String url = "jdbc:Mysql://localhost:3306/BMS";
			String user = "root";
			String pass = "shams817";
			Connection con = DriverManager.getConnection(url, user, pass);
			Statement smt = con.createStatement();
			String q = "Create table Vendor_Id(Vendor_Name varchar(50), Father_Name varchar(50), Contact_Number varchar(20), Email_Id varchar(30), Adhar_No varchar(50), Education_Qualification varchar(50), Experience varchar(50), Address varchar(50), Pin_Code varchar(20), Job_Post varchar(50), Posting_Area varchar(50), Salary varchar(50))";
			smt.execute(q);
			System.out.println("Vendor_Table_Created");
			con.close();

		} catch (Exception e) {
			System.out.println("Table not created  Error " + e);
		}
	}

//---------------------VENDOR_DATA-----------------------------

	public void Vendor_data() {
		try {
			String url = "jdbc:Mysql://localhost:3306/BMS";
			String user = "root";
			String pass = "shams817";
			Connection con = DriverManager.getConnection(url, user, pass);
			Statement smt = con.createStatement();
			String q = "insert into Vendor_Id(Vendor_Name, Father_Name, Contact_Number, Email_Id, Adhar_No, Education_Qualification, Experience, Address, Pin_Code, Job_Post, Posting_Area, Salary)values(?,?,?,?,?,?,?,?,?,?,?,?)";
			PreparedStatement ps = con.prepareStatement(q);
			BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
			System.out.println("Enter the Vendor_Name:");
			String Vendor_Name = br.readLine();
			ps.setString(1, Vendor_Name);

			System.out.println("Enter the Father_Name:");
			String Father_Name = br.readLine();
			ps.setString(2, Father_Name);

			System.out.println("Enter the Contact_Number:");
			String Contact_Number = br.readLine();
			ps.setString(3, Contact_Number);

			System.out.println("Enter the Email_Id:");
			String Email_Id = br.readLine();
			ps.setString(4, Email_Id);

			System.out.println("Enter the Adhar_No:");
			String Adhar_No = br.readLine();
			ps.setString(5, Adhar_No);

			System.out.println("Enter the Education_Qualification:");
			String Education_Qualification = br.readLine();
			ps.setString(6, Education_Qualification);

			System.out.println("Enter the Experience:");
			String Experience = br.readLine();
			ps.setString(7, Experience);

			System.out.println("Enter the Address:");
			String Address = br.readLine();
			ps.setString(8, Address);

			System.out.println("Enter the Pin_Code:");
			String Pin_Code = br.readLine();
			ps.setString(9, Pin_Code);

			System.out.println("Enter the Job_Post:");
			String Job_Post = br.readLine();
			ps.setString(10, Job_Post);

			System.out.println("Enter the Posting_Area:");
			String Posting_Area = br.readLine();
			ps.setString(11, Posting_Area);

			System.out.println("Enter the Salary:");
			String Salary = br.readLine();
			ps.setString(12, Salary);
			ps.executeUpdate();
			System.out.println("New Vendor Id Inserted");
			con.close();

		} catch (Exception e) {
			System.out.println("No Id Inserted  " + e);
		}

	}

//--------------------------VIEW_VENDOR-----------------------------------------

	public void view_Vendor() {
		try {
			String url = "jdbc:Mysql://localhost:3306/BMS";
			String user = "root";
			String pass = "shams817";
			Connection con = DriverManager.getConnection(url, user, pass);

			String q = "select * from Vendor_id where Vendor_Name=?";
			PreparedStatement ps = con.prepareStatement(q);

			BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

			System.out.println("Enter Vendor_Name : ");
			String Vendor_Name = br.readLine();
			ps.setString(1, Vendor_Name);

			ResultSet rs = ps.executeQuery();
			while (rs.next()) {
				System.out.println("Vendor_Name : " + rs.getString(1));
				System.out.println("Father_Name : " + rs.getString(2));
				System.out.println("Contact_Number : " + rs.getString(3));
				System.out.println("Email_Id :" + rs.getString(4));
				System.out.println("Adhar_No : " + rs.getString(5));
				System.out.println("Education_Qualification : " + rs.getString(6));
				System.out.println("Experience : " + rs.getString(7));
				System.out.println("Address :" + rs.getString(8));
				System.out.println("Pin_Code : " + rs.getString(9));
				System.out.println("Job_Post : " + rs.getString(10));
				System.out.println("Posting_Area : " + rs.getString(11));
				System.out.println("Salary :" + rs.getString(12));

				System.out.println(" ");
			}
			System.out.println("Get Data");
			con.close();
		} catch (Exception e) {
			System.out.println("Database access Error " + e);
			admin();
		}

	}
	
	public void Upgrade_KW() {
		Scanner sc = new Scanner(System.in);
		try {
			double bill_amount=0;
			String bill_status = "";
			
			String url = "jdbc:mysql://localhost:3306/bms";
			String user = "root";
			String pass = "shams817";
			
			
			Connection conn = DriverManager.getConnection(url, user, pass);
			
			String q = "SELECT Full_Name, Father_Name, E_mail, Address, Consumer_Number, KW FROM register_new_customer WHERE Consumer_Number=?";
            PreparedStatement pst = conn.prepareStatement(q);
            BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
            System.out.print("Enter your Consumer Number: ");
            String Consumer_Number = br.readLine();
			pst.setString(1, Consumer_Number);
            ResultSet rs = pst.executeQuery();
            
            while (rs.next()) {
            	System.out.println("\nBill Detail");
            	System.out.println("--------------");
            	System.out.println("Consumer_Number: " + rs.getString("Consumer_Number"));
                System.out.println("Full Name: " + rs.getString("Full_Name"));
                System.out.println("Father Name: " + rs.getString("Father_Name"));
                System.out.println("E_mail: " + rs.getString("E_mail"));
                System.out.println("Address: " + rs.getString("Address"));
                System.out.println("Current KW: " + rs.getString("KW"));  
                System.out.println("-------------------------------------");
               
            }
            System.out.println("Enter the new KW: ");
            String new_KW =sc.nextLine();
                 		
                String query = "UPDATE register_new_customer SET KW = ? WHERE Consumer_Number = ?";
                PreparedStatement psmt = conn.prepareStatement(query);
                psmt.setString(1, new_KW);            	
                psmt.setString(2, Consumer_Number);
                psmt.executeUpdate();
                        	
                System.out.println("--New KW was updated successfully.--");
			conn.close();
			
		} catch(Exception e) {
			
			System.out.println("Error" + e);
		}
	}

	public void admin_Dashboard() {
		System.out.println("\nWelcome to Admin Dashboard");
		Scanner sc = new Scanner(System.in);
		System.out.println(" 1. Register_new _customer ");
		System.out.println(" 2. View_Customer ");
		System.out.println(" 3. Generate_Bill  ");
		System.out.println(" 4. View_Bill ");
		System.out.println(" 5. Register_Complain ");
		System.out.println(" 6. View_Complain ");
		System.out.println(" 7. Vender_Register ");
		System.out.println(" 8. View_Vendor ");

		System.out.println(" 0. FOR EXIT ");
		int select;
		System.out.print("Select the option: ");
		select = sc.nextInt();

		if (select == 1) {
			System.out.println("\nRegister_new_Customer: ");
			Insert_customer();
			admin_Dashboard();
		} else if (select == 2) {
			System.out.println("\nView_Customer : ");
			View_Customer();
			admin_Dashboard();
		} else if (select == 3) {
			System.out.println("\nGenerate_new_Bill : ");
			genrate_bill_data();
			admin_Dashboard();

		} else if (select == 4) {
			System.out.println("\nView_Bill : ");
			View_Bill();
			admin_Dashboard();
		} else if (select == 5) {
			System.out.println("\nNew_Complain_Register: ");
			
			admin_Dashboard();
		} else if (select == 6) {
			System.out.println("\nView_Complain: ");
			View_Complain();
			admin_Dashboard();
		} else if (select == 7) {
			System.out.println("\nVendor_Regisertion: ");
			Vendor_data();
			admin_Dashboard();
		} else if (select == 8) {
			System.out.println("\nview Vendor");
			view_Vendor();
			admin_Dashboard();
		} else if (select == 0) {
			System.out.println("EXIT");
			admin();
		}
	}

}
# EBMS
