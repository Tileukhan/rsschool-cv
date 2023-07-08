***FULL NAME***
Tileukhan Seifulla 
***CONTACT INFO***
Tel.number: 87071071477 
***ABOUT ME***
I am a motivated individual eager to pursue new opportunities and expand my knowledge in front-end development.
My main goal is to learn web development and become a front-end developer. I am driven by a strong desire to learn and explore new horizons.
One of my key strengths is creativity and enthusiasm. I am a dedicated and hardworking individual who strives for excellence in everything I do.
Although I may not have extensive professional experience yet, I am enthusiastic about learning and eager to gain practical skills in front-end development.
I have a curious mindset and enjoy keeping up with the latest industry trends and developments.
I believe that a combination of my passion for learning and determination to succeed will enable me to adapt quickly and contribute effectively to any team.
I am committed to personal growth and continuously enhancing my skill set. I embrace new opportunities for self-improvement and actively seek out ways to expand my knowledge base.
I am open to feedback and view it as a valuable tool for growth and development.
In summary, my primary focus is on pursuing new challenges, learning, and staying up-to-date with emerging trends in front-end development. 
***SKILLS***
Programming Languages:
-I have a basic understanding of C++ and Python, and I am proficient in Java at an object-oriented level.
-I have completed projects and assignments in these languages, which have helped me develop a solid foundation.

Version Control Systems:
-I am proficient in using Git, which is a widely adopted version control system. 
I have experience with creating repositories, branching, merging code changes, and collaborating with other developers using Git.

Development Tools: 
-I am comfortable working with integrated development environments (IDEs) such as IntelliJ IDEA for Java development. 
-I have also used Visual Studio Code and PyCharm for C++ and Python projects, respectively. These tools provide a rich set of features that enhance productivity and streamline the development process.
***CODE SAMPLE***
import Headphones.Headphone;
import Laptops.Laptop;
import Markets.*;
import Phones.Phone;

import java.sql.*;
import java.util.ArrayList;
import java.util.Scanner;

public class Main {
    private static final Scanner in = new Scanner(System.in);
    private static final String USER = "postgres";
    private static final String PASS = "1234567";
    private static final String URL = "jdbc:postgresql://localhost:5432/postgres";
    public static Connection connection;

    static {
        try {
            connection = DriverManager.getConnection(URL, USER, PASS);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    public static Statement statement;

    static {
        try {
            statement = connection.createStatement();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    public static void main(String[] args) throws Exception {
        User user = User.getInstance();
        boolean running = true;
        Market Alser = createMarketByName("alser").createMarket();

        Market Tehnodom = createMarketByName("tehnodom").createMarket();

        Market Mechta = createMarketByName("mechta").createMarket();

        System.out.println("Welcome to the our marketplace \"Ozimiz Goi\"!");
        System.out.print("Please enter your name : ");
        user.setName(in.next());
        System.out.print("Now , enter your balance : ");
        user.topUpBalance(in.nextInt());
        System.out.println("Your data was entered. Enjoy with shopping, " + user.getName() + "!");
        while (running) {
            System.out.println("Now , Choose the market : ");
            System.out.println("0. Exit\n1. Alser\n2. Mechta\n3. Tehnodom\n4. Top up my balance");
            int market = in.nextInt();
            if (market == 0) {
                System.out.println("Goodbye!");
                return;
            }
            switch (market) {
                case 1 -> menu(Alser);
                case 2 -> menu(Mechta);
                case 3 -> menu(Tehnodom);
                case 4 -> {
                    System.out.print("Do you have " + user.getBalance() + " dollars, how much do you want to fill it with?");
                    int cash = in.nextInt();
                    user.topUpBalance(cash);
                    System.out.println("The money has been successfully deposited into your account!");
                }

                default -> {
                }
            }
        }
    }

    public static void menu(Market market) throws SQLException {
        User user = User.getInstance();
        System.out.println("What kind of electronic thing u wanna buy?\nIn our shop we have : \n1. Phones\n2. Headphones\n3. Laptops\n0. If you wanna leave");
        int n = in.nextInt();
        while (n > 3 || n < 0) {
            System.out.print("Please, choose the correct one: ");
            n = in.nextInt();
        }
        if (n == 0) {
            return;
        }
        System.out.println("What type you wanna see?");
        if (n == 1) {
            System.out.println("1. Sumsung\n2. Oppo\n3. Iphone\n4. See all\n\n0. Leave");
            int choose = in.nextInt();
            while (choose < 0 || choose > 4) {
                System.out.print("Please, choose the correct variant :");
                choose = in.nextInt();
            }
            ArrayList<Phone> phones;
            if (choose == 1) {
                phones = market.getPhones("Sumsung");
            } else if (choose == 2) {
                phones = market.getPhones("Oppo");
            } else if (choose == 3) {
                phones = market.getPhones("Iphone");
            } else {
                phones = market.getPhones();
            }
            for (Phone phone : phones) {
                System.out.print(phone.info());
            }
            System.out.print("Choose the id of phone : ");
            int id = in.nextInt();
            for (Phone phone : phones) {
                if (phone.getId() == id) {
                    if (user.getBalance() >= phone.getPrice()) {
                        user.buyThing(phone.getPrice());
                        System.out.println("Phone was succesfully bought!");
                        System.out.println("You have "+user.getBalance()+" dollars left");
                        String sql = "delete FROM " + market.getMarketName() + ".phones WHERE id = " + phone.getId();
                        statement.executeUpdate(sql);
                        return;
                    } else {
                        System.out.println("You have not enough money!");
                        return;
                    }
                }
            }
        }
        if (n == 2) {
            System.out.println("1. Airpods\n2. Sony\n3. JBL\n4. See all");
            int choose = in.nextInt();
            ArrayList<Headphone> headphones;
            if (choose == 1) headphones = market.getHeadphones("Airpods");
            else if (choose == 2) headphones = market.getHeadphones("Sony");
            else if (choose == 3) headphones = market.getHeadphones("JBL");
            else headphones = market.getHeadphones();

            for (Headphone headphone : headphones) {
                System.out.println(headphone.info());
            }
            System.out.print("Choose the id of headphone : ");
            int id = in.nextInt();
            for (Headphone headphone : headphones) {
                if (headphone.getId() == id) {
                    if (user.getBalance() >= headphone.getPrice()) {
                        System.out.println("Headphone was succesfully bought!");
                        System.out.println("You have "+user.getBalance()+" dollars left");
                        String sql = "delete FROM " + market.getMarketName() + ".headphones WHERE id = " + headphone.getId();
                        statement.executeUpdate(sql);
                        user.buyThing(headphone.getPrice());
                        return;
                    } else {
                        System.out.println("You not have not enough money!");
                        return;
                    }
                }
            }
            System.out.println("Headphone was not found");
        }
        if (n == 3) {
            System.out.println("1. Acer\n2. Lenovo\n3. Macbook\n4. See all ");
            int choose = in.nextInt();
            ArrayList<Laptop> laptops;
            if (choose == 1) laptops = market.getLaptops("Acer");
            else if (choose == 2) laptops = market.getLaptops("Lenovo");
            else if (choose == 3) laptops = market.getLaptops("Macbook");
            else laptops = market.getLaptops();
            for (Laptop laptop : laptops) {
                System.out.println(laptop.info());
            }
            System.out.print("Choose the id : ");
            int id = in.nextInt();
            for (Laptop laptop : laptops) {
                if (laptop.getId() == id) {
                    if (user.getBalance() >= laptop.getPrice()) {
                        System.out.println("Headphone was succesfully bought!");
                        System.out.println("You have "+user.getBalance()+" dollars left");
                        String sql = "delete FROM " + market.getMarketName() + ".laptops WHERE id = " + laptop.getId();
                        statement.executeUpdate(sql);
                        user.buyThing(laptop.getPrice());
                        return;
                    } else {
                        System.out.println("You not have not enough money!");
                        return;
                    }
                }
            }
            System.out.println("Headphone was not found");
        }
    }
    static MarketFactory createMarketByName(String name) throws Exception {
        if(name.equalsIgnoreCase("alser")){
            return new AlserMarketFactory();
        }
        if(name.equalsIgnoreCase("mechta")){
            return new MechtaMarketFactory();
        }
        if(name.equalsIgnoreCase("tehnodom")){
            return new TehnodomMarketFactory();
        }
        throw new RuntimeException();
    }

}
***EXPERIENCE***
I am just beginner at web development
***EDUCATION***
AITU
***LEVEL OF FOREIGN LANGUAGE(ENGLISH)***
I passes IELTS 2 years ago and receive 6.0 band out of 9.0. This means that my level is upper-intermediate.
