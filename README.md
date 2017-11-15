package e456;

import java.awt.GridLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.ArrayList;
import java.util.List;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPasswordField;
import javax.swing.JTextField;
 
//登录窗体类
public class Main extends JFrame implements ActionListener, Runnable {
    private static final long serialVersionUID = 1L;
    protected JLabel lblPrompt, lblTime, lblUserName, lblPassword;
    protected JTextField txtUserName;
    protected JPasswordField txtPassword;
    protected JButton btnLogin, btnRegister;
    protected static Thread thread = null;
 
    public static void main(String[] args) {
        Data.init();
        Main frmLogin = new Main();
        thread = new Thread(frmLogin);
        thread.start();
    }
     
    public Main() {
        super("用户登录");
         
        initComponent();
    }
     
    //初始化控件
    public void initComponent() {
        lblPrompt = new JLabel("登录剩余时间（秒）：");
        lblTime = new JLabel("60");
        lblUserName = new JLabel("用户名：");
        lblPassword = new JLabel("密    码：");
         
        txtUserName = new JTextField(10);
        txtPassword = new JPasswordField(10);
         
        btnLogin = new JButton("登录");
        btnRegister = new JButton("注册");
         
        btnLogin.addActionListener(this);
        btnRegister.addActionListener(this);
         
        this.setLayout(new GridLayout(4, 2));
        this.add(lblPrompt);
        this.add(lblTime);
        this.add(lblUserName);
        this.add(txtUserName);
        this.add(lblPassword);
        this.add(txtPassword);
        this.add(btnLogin);
        this.add(btnRegister);
         
        txtUserName.setFocusable(true);
         
        this.setSize(400, 200);
        this.setVisible(true);
    }
 
    @Override
    public void actionPerformed(ActionEvent e) {
        JButton btn = (JButton) e.getSource();
         
        if(btn == btnLogin) {
            if(txtUserName.getText().equals("") || txtUserName.getText().trim().equals("")) {
                JOptionPane.showMessageDialog(this, "用户名不能为空！", "登录失败", JOptionPane.ERROR_MESSAGE);
                return;
            }
            if(txtPassword.getText().equals("")) {
                JOptionPane.showMessageDialog(this, "登录密码不能为空！", "登录失败", JOptionPane.ERROR_MESSAGE);
                return;
            }
            String userName = null;
            String password = null;
             
            userName = txtUserName.getText().trim();
            password = txtPassword.getText();
            int i;
             
            for(i=0; i < Data.customers.size(); i++) {
                if(Data.customers.get(i).getUserName().equals(userName) && Data.customers.get(i).getPassword().equals(password)) {
                    break;
                }
            }
             
            if(i < Data.customers.size()) {
                JOptionPane.showMessageDialog(this, "欢迎您，" + userName, "登录成功", JOptionPane.INFORMATION_MESSAGE);
            }
            else {
                JOptionPane.showMessageDialog(this, "登录失败，请检查用户名和密码是否正确......", "登录失败", JOptionPane.ERROR_MESSAGE);
            }
        }
        else if(btn == btnRegister) {
            this.dispose();
            new Register();
        }
    }
 
    @Override
    public void run() {
        while(true) {
            try {
                Thread.sleep(1000);
                int num = Integer.valueOf(lblTime.getText());
                if(num <= 0) {
                    JOptionPane.showMessageDialog(this, "时间到，窗口即将关闭！", "登录失败", JOptionPane.INFORMATION_MESSAGE);
                    System.exit(0);
                }
                else {
                    num--;
                }
                lblTime.setText(String.valueOf(num));
            }
            catch(Exception e) {
                e.printStackTrace();
            }
        }
    }
}
 
//用户注册窗体类
class Register extends JFrame implements ActionListener {
    private static final long serialVersionUID = 1L;
    protected JLabel lblUserName, lblPassword, lblConfirmedPassword;
    protected JTextField txtUserName;
    protected JPasswordField txtPassword, txtConfirmedPassword;
    protected JButton btnRegister, btnReset;
     
    public Register() {
        super("用户注册");
         
        initComponent();
    }
     
    //初始化控件
    public void initComponent() {
        lblUserName = new JLabel("用  户  名：");
        lblPassword = new JLabel("密        码：");
        lblConfirmedPassword = new JLabel("确认密码：");
         
        txtUserName = new JTextField(10);
        txtPassword = new JPasswordField(10);
        txtConfirmedPassword = new JPasswordField(10);
         
        btnReset = new JButton("重置");
        btnRegister = new JButton("注册");
         
        btnReset.addActionListener(this);
        btnRegister.addActionListener(this);
         
        this.setLayout(new GridLayout(4, 2));
        this.add(lblUserName);
        this.add(txtUserName);
        this.add(lblPassword);
        this.add(txtPassword);
        this.add(lblConfirmedPassword);
        this.add(txtConfirmedPassword);
        this.add(btnRegister);
        this.add(btnReset);
         
        txtUserName.setFocusable(true);
         
        this.setSize(400, 200);
        this.setVisible(true);
    }
     
    @Override
    public void actionPerformed(ActionEvent e) {
        JButton btn = (JButton) e.getSource();
         
        if(btn == btnRegister) {
            if(txtUserName.getText().equals("") || txtUserName.getText().trim().equals("")) {
                JOptionPane.showMessageDialog(this, "用户名不能为空！", "注册失败", JOptionPane.ERROR_MESSAGE);
                return;
            }
            if(txtPassword.getText().equals("")) {
                JOptionPane.showMessageDialog(this, "登录密码不能为空！", "注册失败", JOptionPane.ERROR_MESSAGE);
                return;
            }
            if(txtConfirmedPassword.getText().equals("")) {
                JOptionPane.showMessageDialog(this, "确证密码不能为空！", "注册失败", JOptionPane.ERROR_MESSAGE);
                return;
            }
            if(! txtConfirmedPassword.getText().equals(txtPassword.getText())) {
                JOptionPane.showMessageDialog(this, "两次密码必须一致！", "注册失败", JOptionPane.ERROR_MESSAGE);
                return;
            }
             
            String userName = null;
            String password = null;
            int i;
             
            userName = txtUserName.getText().trim();
            password = txtPassword.getText();
             
            for(i=0; i < Data.customers.size(); i++) {
                if(Data.customers.get(i).getUserName().equals(userName)) {
                    break;
                }
            }
             
            if(i < Data.customers.size()) {
                JOptionPane.showMessageDialog(this, "该用户名已经被注册，请选用其他用户名！", "注册失败", JOptionPane.ERROR_MESSAGE);
            }
            else {
                Data.customers.add(new Customer(userName, password));
                JOptionPane.showMessageDialog(this, "恭喜 " + userName + " 注册成功，请牢记您的用户名和密码。\n单击\"确定\"按钮开始登录", "注册成功", JOptionPane.INFORMATION_MESSAGE);
 
                this.dispose();
                new Main();
            }          
        }
        else if(btn == btnReset) {
            txtUserName.setText("");
            txtPassword.setText("");
            txtConfirmedPassword.setText("");
            txtUserName.setFocusable(true);
        }
    }  
}
 
//用户信息类
class Customer {
    protected String userName = null;
    protected String password = null;
     
    public Customer() {
    }
     
    public Customer(String userName, String password) {
        this.userName = userName;
        this.password = password;
    }
     
    public String getUserName() {
        return this.userName;
    }
     
    public void setUserName(String userName) {
        this.userName = userName;
    }
     
    public String getPassword() {
        return this.password;
    }
     
    public void setPassword(String password) {
        this.password = password;
    }
}
 
//缓存用户信息的集合类
class Data {
    public static List<Customer> customers = new ArrayList<Customer>();
     
    public static void init() {
        customers.add(new Customer("fxk", "123456"));
    }
}
