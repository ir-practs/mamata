# mamata
my first repository on github

// java in this code

package JDBC_Project;
import java.awt.*;
import java.awt.event.*;
import java.sql.*;
import javax.swing.*;


public class Insert_Delete1 extends JFrame implements ActionListener
{
    Connection con;
    Statement st;
    ResultSet rs;
    ResultSetMetaData rsmd;
    PreparedStatement ps;
    
    JLabel lblRollno,lblName,lblAddr,lblMob,lblEmail;
    JTextField txtRollno, txtName,txtAddr,txtMob,txtEmail;
    JButton btnInsert,btnDelete,btnDisplay,btnCancel;
    JTextArea txtDisplay;
    
    JPanel pn1,pn2;
    
    Insert_Delete1()
    {
        int r;
        String n,addr;
        setTitle("JDBC PROGRAM");
        setSize(300,400);
        setVisible(true);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        
      lblRollno=new JLabel("Enter Your Roll Number");
      txtRollno=new JTextField(15);
      
      lblName=new JLabel("Enter Your Name");
      txtName=new JTextField(15);
      
      lblAddr=new JLabel("Enter Your Address");
      txtAddr=new JTextField(15);
      
      lblMob=new JLabel("Enter Your Mibile Number");
      txtMob=new JTextField(15);
      
      lblEmail=new JLabel("Enter your Email id");
      txtEmail=new JTextField(15);
        
      txtDisplay=new JTextArea(15,45);
      
      btnInsert=new JButton("Insert");
      btnDisplay=new JButton("Display");
      btnDelete=new JButton("Delete");
      btnCancel=new JButton("Cancel");
      
      pn1=new JPanel();
      pn2=new JPanel();
      
      pn1.setLayout(new GridLayout(5,2));
      
      btnInsert.addActionListener(this);
      btnDelete.addActionListener(this);
      btnCancel.addActionListener(this);
      btnDisplay.addActionListener(this);
      
      pn1.add(lblRollno);
      pn1.add(txtRollno);
      pn1.add(lblName);
      pn1.add(txtName);
      pn1.add(lblAddr);
      pn1.add(txtAddr);
      pn1.add(lblMob);
      pn1.add(txtMob);
      pn1.add(lblEmail);
      pn1.add(txtEmail);
      //pn1.add(btnSubmit);
      pn1.add(btnDelete);
      pn1.add(btnDisplay);
      pn1.add(btnInsert);
      pn1.add(btnCancel);
      
      JScrollPane sp=new JScrollPane(txtDisplay);
      pn2.add(sp);
      
      add(pn1,BorderLayout.NORTH);
      add(pn2,BorderLayout.CENTER);  
    }
    
    
    public void actionPerformed(ActionEvent e){
        try
        {
            Class.forName("com.mysql.jdbc.Driver");
        }
        catch(ClassNotFoundException e1)
        {
            System.out.println(e1);
        }
        try
        {
            String url="jdbc:mysql://localhost:3306/emp_1";//dsn
            con=DriverManager.getConnection(url,"root","12345");
            
            st=con.createStatement();
            
        }
        catch(SQLException e2){
            
        }
        if(e.getSource()==btnDisplay){
            try
            {
                int r=Integer.parseInt(txtRollno.getText());
                String n=txtName.getText();
                String addr=txtAddr.getText();
                int M=Integer.parseInt(txtMob.getText());
                String E=txtEmail.getText();
                
                rs=st.executeQuery("select * from emp_11");
                rsmd=rs.getMetaData();
                int cols=rsmd.getColumnCount();
                
                txtDisplay.setText("");
                for(int i=1; i<cols;i++)
                {
                    txtDisplay.append(rsmd.getColumnName(i)+"\t");
                }
                txtDisplay.append("\n");
                while(rs.next())
                {
                    txtDisplay.append("\n"+rs.getInt(1)+"\t"+rs.getString(2)+"\t"+rs.getString(3)+"\t"+rs.getInt(4)+"\t"+rs.getString(5)+"\t");
                    
                }
                
            }
            catch(SQLException e3)
            {
                
            }
        }
        if(e.getSource()==btnInsert){
            try{
                int r=Integer.parseInt(txtRollno.getText());
                String n=txtName.getText();
                String addr=txtAddr.getText();
                int M=Integer.parseInt(txtMob.getText());
                String E=txtEmail.getText();
                
                String str="insert into emp_11 values(?,?,?,?,?)";
                ps=con.prepareStatement(str);
                
                ps.setInt(1, r);
                ps.setString(2,n);
                ps.setString(3, addr);
                ps.setInt(4,M);
                ps.setString(5, E);
                
                ps.executeUpdate();
                
                
                
            }
            catch(SQLException e4){
                
            }
            
        }
        if(e.getSource()==btnDelete)
        {
            try{
                
            
            String str=JOptionPane.showInputDialog(this,"Enter Rollno");
            int r=Integer.parseInt(str);
            
            String sql="delete from emp_11 where Emp_Id=?";
            ps=con.prepareStatement(sql);
            
            ps.setInt(1, r);
            ps.executeUpdate();
        }
        catch(SQLException e5)
        {
            
        }
               
        }
        if(e.getSource()==btnCancel){
            System.exit(0);
        }
        
        
    }
    
 public static void main(String args[])
        {
            new JDBC_Connection();
        }
    
}
