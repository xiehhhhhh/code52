# code52
import java.awt.BorderLayout;
import java.awt.Dimension;
import java.awt.FileDialog;
import java.awt.Font;
import javax.swing.Box;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JTextField;

  
public class Main {
    Sentor sentor = null;
    public static void main(String[] args){
        (new Main()).GUI();
    }

    public void GUI(){
        Font font = new Font("HarmonyOS Sans SC",Font.PLAIN,20);
        JFrame main = new JFrame("Uploader");
        main.setFont(font);
        
        
        Box hbox = Box.createHorizontalBox();

        hbox.add(Box.createHorizontalGlue());
        JLabel ipAddress = new JLabel("Target");
        ipAddress.setFont(font);
        hbox.add(ipAddress);
        hbox.add(Box.createHorizontalStrut(10));
        
        JTextField jtf = new JTextField();
        jtf.setFont(font);
        jtf.setText("localhost");
        jtf.setMaximumSize(new Dimension(180,28));
        jtf.setPreferredSize(new Dimension(180, 28));
        hbox.add(jtf);

        JTextField jtf2 = new JTextField();
        jtf2.setMaximumSize(new Dimension(90,28));
        jtf2.setPreferredSize(new Dimension(90,28));
        jtf2.setText("8080");
        jtf2.setFont(font);
        hbox.add(jtf2);
        hbox.add(Box.createHorizontalStrut(10));

        JButton connect  = new JButton("connect");
        connect.setMaximumSize(new Dimension(150,28));
        connect.setPreferredSize(new Dimension(150,28));
        connect.setFont(font);
        
        hbox.add(connect);
        hbox.add(Box.createHorizontalGlue());

        Box vbox = Box.createVerticalBox();

        Box hbox2 = Box.createHorizontalBox();
        hbox2.add(Box.createHorizontalGlue());
        JLabel status = new JLabel("Not Connected");
        status.setFont(font);
        hbox2.add(status);
        hbox2.add(Box.createHorizontalGlue());

        connect.addActionListener(e->{
            Sentor sentor = new Sentor(jtf.getText(), jtf2.getText());
            System.out.println("try");
            if(sentor.connect()){
                System.out.println("yes");
                status.setText("Connected");
                this.sentor = sentor;
            }else{
                status.setText("Error");
            }
        });

        Box hbox3 = Box.createHorizontalBox();
        hbox3.add(Box.createHorizontalGlue());
        
        JButton choose = new JButton("Choose");
        choose.setFont(font);
        choose.setPreferredSize(new Dimension(200,40));
        choose.setMaximumSize(new Dimension(200,40));
        hbox3.add(choose);
        hbox3.add(Box.createHorizontalStrut(30));

        JTextField jtf3 = new JTextField();
        jtf3.setFont(font);
        jtf3.setMaximumSize(new Dimension(300,28));
        jtf3.setPreferredSize(new Dimension(300,28));
        jtf3.setEditable(false);
        hbox3.add(jtf3);
        hbox3.add(Box.createHorizontalStrut(15));

        JButton send = new JButton("Send");
        send.setFont(font);
        send.setPreferredSize(new Dimension(100,28));
        send.setMaximumSize(new Dimension(100,28));
        hbox3.add(send);
        hbox3.add(Box.createHorizontalGlue());

        choose.addActionListener(e->{
            FileDialog fd = new FileDialog(main,"Choose");
            fd.setVisible(true);
            jtf3.setText(fd.getDirectory()+fd.getFile());
        });

        send.addActionListener(e->{
            if(this.sentor !=null){
                String filePath = jtf3.getText();
                if(filePath==null||filePath.equals("")){
                    JOptionPane.showMessageDialog(main,"Illegal FilePath","info",JOptionPane.WARNING_MESSAGE);
                    return ;
                }
                Boolean returns = this.sentor.Sent(jtf3.getText());
                if(returns){
                    status.setText("Okay");
                }else{
                    status.setText("Failed");
                }
            }else{
                JOptionPane.showMessageDialog(main,"Not Connected","info",JOptionPane.WARNING_MESSAGE);
            }
        });
        
        vbox.add(Box.createVerticalGlue());
        vbox.add(hbox);
        vbox.add(Box.createVerticalStrut(15));
        vbox.add(hbox2);
        vbox.add(Box.createVerticalStrut(15));
        vbox.add(hbox3);
        vbox.add(Box.createHorizontalGlue());

        main.add(vbox,BorderLayout.CENTER);
        main.setSize(new Dimension(600,200));
        main.setLocationRelativeTo(null); // CENTER
        main.setVisible(true);
        // jtf.setText("localhost");
        // jtf2.setText("8080");
    }
}
