# Smart-Banking-System




import java.util.*;
import java.io.*;

public class BankAccount {
    //Creating global Scanner object
    public static Scanner scr = new Scanner(System.in);

    //Data members
    static int id = 1000; //for unique ID
    String uid;
    String name;
    String address;
    String type;
    double balance;
    public static int no_of_trans = 0;
    private List<String> transactionHistory = new ArrayList<>();

    //Default Constructor
    BankAccount() {
        uid = null;
        name = address = type = null;
        balance = 0.0;
    }

    BankAccount(String name, String address, String type, double balance) {
        uid = UID();
        this.name = name;
        this.address = address;
        this.type = type;
        this.balance = balance;
        addTransaction("Account Created with Initial Balance ₹" + balance);
    }

    //Generate unique ID
    String UID() {
        return "BA" + Integer.toString(id++);
    }

    //Display info and balance of depositor
    void display() {
        System.out.println("");
        System.out.println("|\t***ACCOUNT INFORMATION***");
        System.out.println("|");
        System.out.println("|     Name of Holder    : " + this.name);
        System.out.println("|     Account No        : " + this.uid);
        System.out.println("|     Account Type      : " + this.type);
        System.out.println("|     Available Balance : ₹" + this.balance);
        System.out.println("|     Address of Holder : " + this.address);
        System.out.println("|");
        System.out.println("");
    }

    //Add transaction record
    void addTransaction(String transaction) {
        transactionHistory.add(transaction);
        no_of_trans++;
    }

    //Print transaction history
    void printTransactionHistory() {
        System.out.println("Transaction History for Account No: " + this.uid);
        for (String transaction : transactionHistory) {
            System.out.println(transaction);
        }
        System.out.println("");
    }

    //Deposit amount in account
    void deposit() {
        System.out.println("");
        System.out.println("Account No : " + this.uid);
        System.out.println("Name : " + this.name);
        System.out.print("Enter amount to be Deposited : ₹");
        double blnc = Double.parseDouble((scr.nextLine()));
        this.balance += blnc;
        addTransaction("Deposited ₹" + blnc);
        System.out.println("");
        System.out.println("Amount Credited Successfully...");
        System.out.println("");
    }

    //Withdraw from account
    void withdraw() {
        System.out.println("");
        System.out.println("Account No : " + this.uid);
        System.out.println("Name : " + this.name);
        System.out.print("Enter amount to be Withdrawn : ₹");
        double blnc = Double.parseDouble((scr.nextLine()));
        if (blnc > this.balance) {
            System.out.println("Insufficient funds!");
        } else {
            this.balance -= blnc;
            addTransaction("Withdrew ₹" + blnc);
            System.out.println("");
            System.out.println("Amount Debited Successfully...");
        }
        System.out.println("");
    }

    //Transfer funds between accounts
    void transfer(BankAccount recipient) {
        System.out.println("");
        System.out.println("Account No (Sender) : " + this.uid);
        System.out.println("Name : " + this.name);
        System.out.print("Enter amount to transfer : ₹");
        double transferAmount = Double.parseDouble(scr.nextLine());
        if (this.balance < transferAmount) {
            System.out.println("Insufficient funds to complete the transfer.");
        } else {
            this.balance -= transferAmount;
            recipient.balance += transferAmount;
            addTransaction("Transferred ₹" + transferAmount + " to Account No: " + recipient.uid);
            recipient.addTransaction("Received ₹" + transferAmount + " from Account No: " + this.uid);
            System.out.println("Transfer of ₹" + transferAmount + " completed successfully.");
        }
        System.out.println("");
    }

    //Change address of depositor
    void changeAddress() {
        System.out.println("");
        System.out.println("Account No : " + this.uid);
        System.out.println("Name : " + this.name);
        System.out.print("Enter New Address : ");
        this.address = scr.nextLine();
        addTransaction("Address changed to: " + this.address);
        System.out.println("");
        System.out.println("Address Successfully Changed...");
        System.out.println("");
    }

    //Apply interest to savings account (For simplicity, let's assume interest rate of 4%)
    void applyInterest() {
        if (this.type.equalsIgnoreCase("Savings")) {
            double interest = this.balance * 0.04;
            this.balance += interest;
            addTransaction("Interest of ₹" + interest + " applied to the account.");
            System.out.println("Interest of ₹" + interest + " has been applied to the account.");
        } else {
            System.out.println("Interest can only be applied to Savings accounts.");
        }
        System.out.println("");
    }

    //Close the account
    void closeAccount() {
        System.out.println("Account No: " + this.uid + " has been closed.");
        addTransaction("Account Closed with Balance ₹" + this.balance);
        this.balance = 0.0; // Reset balance
        this.uid = null; // Remove the UID
        this.name = null; // Remove the name
        this.address = null; // Remove the address
        this.type = null; // Remove the account type
    }

    public static void main(String args[]) {
        //Enter no. of depositors
        System.out.print("Enter Numbers of depositors : ");
        int n = Integer.parseInt(scr.nextLine());

        //Creating array of objects(allocating memory)
        BankAccount[] bk = new BankAccount[n];

        //Enter information of 'n' depositors
        for (int i = 0; i < n; i++) {
            System.out.println("Enter Information for Depositor no. " + (i + 1));
            System.out.print("Enter name : ");
            String name = scr.nextLine();
            System.out.print("Type of Account (Savings/Current) : ");
            String type = scr.nextLine();
            System.out.print("Enter Address : ");
            String address = scr.nextLine();
            System.out.print("Enter Balance : ₹");
            double balance = Double.parseDouble(scr.nextLine());
            //Initialize elements of array object
            bk[i] = new BankAccount(name, address, type, balance);
            System.out.println("");
        }

        int num; //hold depositor no. to be manipulated

        //flag bit to exit from the loop
        int flag = 1;
        do {
            System.out.println("");
            System.out.println("MENU FOR ACCOUNT MANIPULATION");
            System.out.println("1. Print information of any depositor");
            System.out.println("2. Add amount to the account of any depositor");
            System.out.println("3. Remove some amount from account of any depositor");
            System.out.println("4. Change Address of any depositor");
            System.out.println("5. Transfer funds between two accounts");
            System.out.println("6. Apply interest to a Savings account");
            System.out.println("7. Close an account");
            System.out.println("8. Print transaction history of any depositor");
            System.out.println("9. Print total number of transactions");
            System.out.println("10. Exit the program...");
            System.out.print("Enter your Choice : ");
            int choice = Integer.parseInt(scr.nextLine());
            System.out.println("");

            switch (choice) {
                case 1:
                    System.out.print("Depositor no. : ");
                    num = Integer.parseInt(scr.nextLine());
                    bk[num - 1].display();
                    break;
                case 2:
                    System.out.print("Depositor no. : ");
                    num = Integer.parseInt(scr.nextLine());
                    bk[num - 1].deposit();
                    break;
                case 3:
                    System.out.print("Depositor no. : ");
                    num = Integer.parseInt(scr.nextLine());
                    bk[num - 1].withdraw();
                    break;
                case 4:
                    System.out.print("Depositor no. : ");
                    num = Integer.parseInt(scr.nextLine());
                    bk[num - 1].changeAddress();
                    break;
                case 5:
                    System.out.print("Sender Depositor no. : ");
                    int sender = Integer.parseInt(scr.nextLine());
                    System.out.print("Recipient Depositor no. : ");
                    int recipient = Integer.parseInt(scr.nextLine());
                    bk[sender - 1].transfer(bk[recipient - 1]);
                    break;
                case 6:
                    System.out.print("Depositor no. : ");
                    num = Integer.parseInt(scr.nextLine());
                    bk[num - 1].applyInterest();
                    break;
                case 7:
                    System.out.print("Depositor no. : ");
                    num = Integer.parseInt(scr.nextLine());
                    bk[num - 1].closeAccount();
                    break;
                case 8:
                    System.out.print("Depositor no. : ");
                    num = Integer.parseInt(scr.nextLine());
                    bk[num - 1].printTransactionHistory();
                    break;
                case 9:
                    System.out.println("Total number of transactions today : " + no_of_trans);
                    break;
                case 10:
                    System.out.println("Exiting...");
                    flag = 0;
                    break;
                default:
                    System.out.println("Wrong Choice!!!");
            }
        } while (flag == 1);
    }
}
