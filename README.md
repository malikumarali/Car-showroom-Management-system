# Car-showroom-Management-system
We created a Car showroom management system using Java for programming fundamentals.
import java.util.*;
import java.io.*;

public class SSM {
    static final String FILE_NAME = "showroom_cars.txt"; // File to store car records
    static final String SENT = "No"; // Constant for user input comparison
    static String[] name = new String[1000];
    static String[] brand = new String[1000];
    static String[] model = new String[1000];
    static String[] engine = new String[1000];
    static double[] price = new double[1000];
    static int carCount = 0; // Counter for the number of cars

    public static void main(String[] args) {
        boolean admin = false;                                                                    
        while (true){
            try (Scanner input = new Scanner(System.in)) {                                      
                loadCarsFromFile(); 
                addPredefinedCars();                                                             

                while (true) {
                    int role = selectRole(input); 

                    if (role == 1) { 
                        admin = adminDaRola();
                        if (admin){
                        handleAdminPanel(input);
                    }

                    } else if (role == 2) { 
                        handleCustomerPanel(input);

                    } else if (role == 3){
                        System.out.println();
                        return; 
                    }
                }
            } catch (Exception e) {
                System.out.println("Program terminated unexpectedly: " + e.getMessage());
            }
        }
    }

    // Prompt user to select their role
    public static int selectRole(Scanner input) {
        int role;
        while (true) {
            try {
                System.out.println("=========================================");
                System.out.println("          Welcome to the Showroom       ");
                System.out.println("=========================================");
                System.out.println("Please select your role:");
                System.out.println("1. Admin");
                System.out.println("2. Customer");
                System.out.println("3. Exit");
                System.out.println("=========================================");
                System.out.print("Enter your choice: ");
                role = input.nextInt();
                System.out.println();

                if (role >= 1 && role <= 3) {
                    break;
                } else {
                    System.out.println("Invalid input. Please try again.");
                }
            } catch (InputMismatchException e) {
                System.out.println("You can't enter that option. Please try again.");
                input.nextLine(); // Consume the invalid input
            }
        }
        return role;
    }

    // Handle Admin Panel
    private static void handleAdminPanel(Scanner input) {
        boolean access = true;
        while (access) {
            try {
                System.out.println("\n========================================");
                System.out.println("        Welcome to the Admin Panel       ");
                System.out.println("========================================");
                System.out.println("Please select an option:");
                System.out.println("  1.  Showroom Module");
                System.out.println("  2.  Car Module");
                System.out.println("  3.  Buy Module");
                System.out.println("  4.  Read Feedback");
                System.out.println("  5.  Return to Main Menu");
                System.out.print("Your choice: ");

                int choice = input.nextInt();
                input.nextLine();

                switch (choice) {
                    case 1 -> showroomModule(input);
                    case 2 -> carModule(input);
                    case 3 -> buyModule();
                    case 4 -> feedback_reader(input);
                    case 5 -> {
                        System.out.println("\nReturning to Main Menu...");
                        access = false; // Exit Admin Panel loop
                    }
                    default -> System.out.println("Invalid option. Please choose a valid number.");
                }
            } catch (InputMismatchException e) {
                System.out.println("Invalid input. Please enter a valid number.");
                input.nextLine(); // Clear invalid input
            } catch (Exception e) {
                System.out.println("Error: " + e.getMessage());
            }
        }
    }

    // Handle Customer Panel
    private static void handleCustomerPanel(Scanner input) {
        boolean custa = true;
        while (custa) {
            try {
                System.out.println("");
                System.out.println("*        Welcome to Our Store!           *");
                System.out.println("");
                System.out.println("How can we serve you?");
                System.out.println("1. To Buy Car");
                System.out.println("2. To Sell a Car");
                System.out.println("3. To Report Feedback");
                System.out.println("4. Return to Main Menu");
                System.out.print("Enter your choice: ");

                int cust = input.nextInt();
                switch (cust) {
                    case 1 -> buyModule();
                    case 2 -> sellCarToSR(input);
                    case 3 -> {
                        System.out.print("Your Opinion here: ");
                        input.nextLine();
                        String feedback = input.nextLine();
                        feedbackW(feedback);
                    }
                    case 4 -> {
                        System.out.println("\nReturning to Main Menu...");
                        custa = false; // Exit Customer Panel loop
                    }
                    default -> System.out.println("Invalid entry. Please try again.");
                }
            } catch (InputMismatchException e) {
                System.out.println("Invalid input. Please enter a valid number.");
                input.nextLine(); // Clear invalid input
            }
        }
    }


    public static void showroomModule(Scanner input) {
        while (true) {
            System.out.println("\nShowroom Module - Select an option:");
            System.out.println("1. Add a Car");
            System.out.println("2. Sell a Car");
            System.out.println("3. Display All Cars");
            System.out.println("4. Exit");

            try {
                int choice = input.nextInt();
                input.nextLine(); // Consume the newline character

                switch (choice) {
                    case 1 -> addCarToShowroom(input);
                    case 2 -> sellCar(input);
                    case 3 -> displayCars();
                    case 4 -> {
                                System.out.println("Exiting Showroom Module...");
                        return;
                    }
                    default -> System.out.println("Invalid option. Please try again.");
                }
            } catch (InputMismatchException e) {
                System.out.println("Invalid input. Please enter a valid number.");
                input.nextLine();
            }
        }
    }

    public static void addCarToShowroom(Scanner input) {
        while (true) {
            if (carCount >= name.length) {
                System.out.println("Showroom is full. Cannot add more cars.");
                break;
            }
            try {
                System.out.println("Enter Car Details:");

                String carName;
                while (true) {
                    System.out.print("Name: ");
                    carName = input.nextLine();
                    if (!carName.isEmpty()) {
                        break; 
                    }
                    System.out.println("Name cannot be empty. Please enter a valid name.");
                }
                name[carCount] = carName;

        
                String carBrand;
                while (true) {
                    System.out.print("Brand: ");
                    carBrand = input.nextLine();
                    if (!carBrand.isEmpty()) {
                        break; 
                    }
                    System.out.println("Brand cannot be empty. Please enter a valid brand.");
                }
                brand[carCount] = carBrand;

                // Get car model
                String carModel;
                while (true) {
                    System.out.print("Model: ");
                    carModel = input.nextLine();
                    if (!carModel.isEmpty()) {
                        break; // Valid input
                    }
                    System.out.println("Model cannot be empty. Please enter a valid model.");
                }
                model[carCount] = carModel;

                // Get car engine
                String carEngine;
                while (true) {
                    System.out.print("Engine: ");
                    carEngine = input.nextLine();
                    if (!carEngine.isEmpty()) {
                        break; // Valid input
                    }
                    System.out.println("Engine cannot be empty. Please enter a valid engine.");
                }
                engine[carCount] = carEngine;

                // Read price and ensure it's a valid double
                double carPrice;
                while (true) {
                    System.out.print("Price (in millions): ");
                    String priceInput = input.nextLine(); // Read price as a string
                    if (priceInput.isEmpty()) {
                        System.out.println("Price cannot be empty. Please enter a valid price.");
                        continue; // Prompt again
                    }
                    try {
                        carPrice = Double.parseDouble(priceInput); // Convert to double
                        if (carPrice < 0) {
                            System.out.println("Price cannot be negative. Please enter a valid price.");
                            continue; // Prompt again
                        }
                        break; // Valid price
                    } catch (NumberFormatException e) {
                        System.out.println("Price must be a numeric value. You entered: " + priceInput);
                    }
                }
                price[carCount] = carPrice;

                carCount++; // Increment the car count
                saveCarsToFile(); // Save to file after adding a car
                System.out.println("Car added successfully.");
            } catch (Exception e) {
                System.out.println("Invalid entry: " + e.getMessage());
            }

            // Ask if the user wants to continue adding cars
            System.out.print("Do you want to continue adding cars? (Yes/No): ");
            String cont = input.nextLine();
            if (cont.equalsIgnoreCase("No")) {
                break; // Exit the loop if the user says "No"
            }
        }
    }

    public static void sellCar(Scanner input) {
        if (carCount == 0) {
            System.out.println("No cars available to sell.");
            return;
        }

        displayCars(); // Display cars before selling

        while (true) {
            try {
                System.out.print("Enter the index of the car you want to sell (starting from 0): ");
                int index = input.nextInt();
                input.nextLine(); // Consume the newline character

                if (index >= 0 && index < carCount) {
                    for (int i = index; i < carCount - 1; i++) {
                        name[i] = name[i + 1];
                        brand[i] = brand[i + 1];
                        model[i] = model[i + 1];
                        engine[i] = engine[i + 1];
                        price[i] = price[i + 1];
                    }
                    carCount--; // Decrement car count
                    saveCarsToFile(); // Save to file after selling a car
                    System.out.println("Car sold successfully.");
                    return;
                } else {
                    System.out.println("Invalid index. Please try again.");
                }
            } catch (InputMismatchException e) {
                System.out.println("Invalid input. Please enter a valid number.");
                input.nextLine(); // Clear invalid input
            }
        }
    }

    public static void displayCars() {
        if (carCount == 0) {
            System.out.println("No cars available.");
            return;
        }

        System.out.println("\nCars in the showroom:");
        for (int i = 0; i < carCount; i++) {
            System.out.printf("%d: Name: %s, Brand: %s, Model: %s, Engine: %s, Price: %.2fM%n",
                    i, name[i], brand[i], model[i], engine[i], price[i]);
        }
    }

    public static void carModule(Scanner input) {
        while (true) {
            System.out.println("\nCar Module - Select an option:");
            System.out.println("1. View Car Details");
            System.out.println("2. Update Car Information");
            System.out.println("3. Search for Cars");
            System.out.println("4. Exit");

            try {
                int choice = input.nextInt();
                input.nextLine(); // Consume the newline character

                switch (choice) {
                    case 1 -> viewCarDetails(input);
                    case 2 -> updateCarInfo(input);
                    case 3 -> searchCars(input);
                    case 4 -> {
                        System.out.println("Exiting Car Module...");
                        return; // Exit the car module
                    }
                    default -> System.out.println("Invalid option. Please try again.");
                }
            } catch (InputMismatchException e) {
                System.out.println("Invalid input. Please enter a valid number.");
                input.nextLine(); // Clear invalid input
            }
        }
    }

    public static void viewCarDetails(Scanner input) {
        System.out.print("Enter the index of the car to view (starting from 0): ");
        try {
            int index = input.nextInt();
            input.nextLine(); // Consume the newline character

            if (index >= 0 && index < carCount) {
                System.out.printf("""
                        -----------------------------------------
                                         Car Details                     
                        -----------------------------------------
                        Name   : %s
                        Brand  : %s
                        Model  : %s
                        Engine : %s
                        Price  : %.2f Million
                        -----------------------------------------
                        """, name[index], brand[index], model[index], engine[index], price[index]);
            } else {
                System.out.println("Invalid index. Car not found.");
            }
        } catch (InputMismatchException e) {
            System.out.println("Invalid input. Please enter a valid number.");
            input.nextLine(); // Clear invalid input
        }
    }

    public static void updateCarInfo(Scanner input) {
        System.out.print("Enter the index of the car to update (starting from 0): ");
        try {
            int index = input.nextInt();
            input.nextLine(); // Consume the newline character

            if (index >= 0 && index < carCount) {
                System.out.println("Enter new details for the car:");

                System.out.print("Name: ");
                name[index] = input.nextLine();

                System.out.print("Brand: ");
                brand[index] = input.nextLine();

                System.out.print("Model: ");
                model[index] = input.nextLine();

                System.out.print("Engine Type: ");
                engine[index] = input.nextLine();

                System.out.print("Price (in millions): ");
                price[index] = input.nextDouble();
                input.nextLine(); // Consume the newline character

                System.out.println("Car details updated successfully.");
                saveCarsToFile(); // Save to file after updating
            } else {
                System.out.println("Invalid index. Car not found.");
            }
        } catch (InputMismatchException e) {
            System.out.println("Invalid input. Please enter valid data.");
            input.nextLine(); // Clear invalid input
        }
    }

    public static void searchCars(Scanner input) {
        System.out.print("Which Car do you want to buy? Try a guess :): ");
        String criteria = input.nextLine().toLowerCase();
        boolean found = false;

        System.out.println("Search Results:");
        for (int i = 0; i < carCount; i++) {
            if (brand[i].toLowerCase().contains(criteria) ||
                    model[i].toLowerCase().contains(criteria) ||
                    engine[i].toLowerCase().contains(criteria) ||
                    String.valueOf(price[i]).contains(criteria)) {
                System.out.printf("%d: Name: %s, Brand: %s, Model: %s, Engine: %s, Price: %.2fM%n",
                        i, name[i], brand[i], model[i], engine[i], price[i]);
                found = true;
            }
        }

        if (!found) {
            System.out.println("No cars found matching the search criteria.");
        }
    }

    public static void addPredefinedCars() {
        // Add some predefined cars
        name[carCount] = "Rolex";
        brand[carCount] = "Tesla";
        model[carCount] = "2022";
        engine[carCount] = "Petrol";
        price[carCount] = 20.0;
        carCount++;

        name[carCount] = "Jim";
        brand[carCount] = "BMW";
        model[carCount] = "2020";
        engine[carCount] = "Hybrid";
        price[carCount] = 25.0;
        carCount++;
    }

    public static int searchCartoBuy(Scanner input) {
        System.out.print("Which Car do you want to buy? Try a guess :) ");
        String criteria = input.nextLine().toLowerCase();
        boolean found = false;
    
        System.out.println("Search Results:");
        for (int i = 0; i < carCount; i++) {
            if (name[i].toLowerCase().contains(criteria) ||brand[i].toLowerCase().contains(criteria) ||
                    model[i].toLowerCase().contains(criteria) ||
                    engine[i].toLowerCase().contains(criteria) ||
                    String.valueOf(price[i]).contains(criteria)) {
                System.out.printf("%d: Name: %s, Brand: %s, Model: %s, Engine: %s, Price: %.2fM%n",
                        i, name[i], brand[i], model[i], engine[i], price[i]);
                found = true;
            }
        }
    
        if (!found) {
            System.out.println("No cars found matching the search criteria.");
            return -1; 
        }
    
        int index = -1;
        while (index == -1) {
            System.out.print("Enter the index of the car you want to buy: ");
            try {
                index = input.nextInt();
                input.nextLine(); 
    
                if (index < 0 || index >= carCount) {
                    System.out.println("Invalid index. Please try again.");
                    index = -1; 
                }
            } catch (InputMismatchException e) {
                System.out.println("Invalid input. Please enter a valid number.");
                input.nextLine(); 
            }
        }
        return index; 
    }
    
    public static void buyModule() {
        Scanner input = new Scanner(System.in);
        int carIndex = searchCartoBuy(input); 
    
        if (carIndex == -1) {
            System.out.println("No car found matching your search.");
            return;  
        }
    
        System.out.println("Car Found: ");
        System.out.println("Name: " + name[carIndex]);
        System.out.println("Brand: " + brand[carIndex]);
        System.out.println("Model: " + model[carIndex]);
        System.out.println("Engine: " + engine[carIndex]);
        System.out.println("Price: " + price[carIndex] + " million");
    
        String buyChoice = "";
        while (!buyChoice.equalsIgnoreCase("yes") && !buyChoice.equalsIgnoreCase("no")) {
            System.out.print("Do you want to buy this car? (yes/no): ");
            buyChoice = input.nextLine();
            if (buyChoice.equalsIgnoreCase("yes")) {
                proceedToBuy(input, carIndex);
            } else if (buyChoice.equalsIgnoreCase("no")) {
                System.out.println("Car purchase cancelled.");
                return; 
            } else {
                System.out.println("Invalid input. Please enter 'yes' or 'no'.");
            }
        }
    }
    
    public static void proceedToBuy(Scanner input, int carIndex) {
        double customerMoney = -1; 
    
        while (customerMoney < 0) { 
            try {
                System.out.print("Enter the amount of money you have (in millions): ");
                customerMoney = input.nextDouble();  
                input.nextLine();  
                if (customerMoney < 0) {
                    System.out.println("Money cannot be negative. Please try again.");
                }
            } catch (InputMismatchException e) {
                System.out.println("Invalid input. Please enter a valid numeric value.");
                input.nextLine();  
            }
        }
    
        if (customerMoney >= price[carIndex]) {
            System.out.println("Congratulations! You have successfully bought the car: " + name[carIndex]);
    
            
            for (int i = carIndex; i < carCount - 1; i++) {
                name[i] = name[i + 1];
                brand[i] = brand[i + 1];
                model[i] = model[i + 1];
                engine[i] = engine[i + 1];
                price[i] = price[i + 1];
            }
            carCount--;  
            saveCarsToFile(); 
        } else {
            String tryAgain = "";
            while (!tryAgain.equalsIgnoreCase("exit") && !tryAgain.equalsIgnoreCase("try")) {
                System.out.println("You don't have enough money to buy this car.");
                System.out.print("Would you like to try again or go back to the main menu? (try/exit): ");
                tryAgain = input.nextLine();
                if (tryAgain.equalsIgnoreCase("try")) {
                    proceedToBuy(input, carIndex);  
                } else if (tryAgain.equalsIgnoreCase("exit")) {
                    System.out.println("Returning to the main menu...");
                    return; 
                } else {
                    System.out.println("Invalid input. Please enter 'try' or 'exit'.");
                }
            }
        }
    }

    public static boolean entry(String username, String password) {
        String[] admin = {"umar007", "ahsan19", "aizaz420"};
        String[] pass = {"umar", "ahsan", "aizaz"};
        boolean ex = false;
        for (int i = 0; i < admin.length; i++) {
            if (admin[i].equals(username) && pass[i].equals(password)) {
                System.out.println("Access Granted");
                ex = true;
                return ex;
            }
        }

        if (!ex) {
            System.out.println("Invalid username or password. Access Denied.");
            return ex;
        }
        return ex;
    }

    public static boolean adminDaRola() {
        Scanner input = new Scanner(System.in);
        boolean access = false;
        System.out.println();
        System.out.println("Enter Credentials");
        System.out.println("------------------------------------------");
        System.out.print("Enter Admin username: ");
        String username = input.nextLine();
        System.out.print("Enter Password: ");
        String password = input.nextLine();
        access = entry(username, password);
        while (!access) {
            System.out.println("Do you want to try again? ");
            String again = input.nextLine();
            System.out.println("-------------------------------------------");
            if (again.equalsIgnoreCase(SENT)) {
                System.out.println("There you go.. Exiting"); 
                break;

            } else {
                System.out.println();
                System.out.println("Enter Credentials");
                System.out.println("-------------------------------------------");
                System.out.print("Enter Admin username: ");
                username = input.nextLine();
                System.out.print("Enter Password: ");
                password = input.nextLine();
                access = entry(username, password);
            }
        }
        return access;
    }

    public static void saveCarsToFile() {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(FILE_NAME))) {
            for (int i = 0; i < carCount; i++) {
                writer.write(name[i] + "," + brand[i] + "," + model[i] + "," + engine[i] + "," + price[i]);
                writer.newLine();
            }
            System.out.println("Car records saved to file.");
        } catch (IOException e) {
            System.out.println("Error saving car records: " + e.getMessage());
        }
    }

    public static void loadCarsFromFile() {
        File file = new File(FILE_NAME);

        // Check if file exists, if not create it
        try {
            if (!file.exists()) {
                System.out.println("File not found. Creating a new file...");
                if (file.createNewFile()) {
                    System.out.println("File created: " + FILE_NAME);
                } else {
                    System.out.println("Failed to create the file.");
                }
            } else {
                // Load cars from the file
                try (Scanner fileReader = new Scanner(file)) {
                    while (fileReader.hasNextLine()) {
                        String line = fileReader.nextLine();
                        String[] carData = line.split(","); // Assuming CSV format: name,brand,model,engine,price

                        if (carData.length == 5) {
                            name[carCount] = carData[0];
                            brand[carCount] = carData[1];
                            model[carCount] = carData[2];
                            engine[carCount] = carData[3];
                            price[carCount] = Double.parseDouble(carData[4]);
                            carCount++;
                        }
                    }
                }
            }
        } catch (IOException e) {
            System.out.println("An error occurred while loading the file: " + e.getMessage());
        } catch (NumberFormatException e) {
            System.out.println("Error parsing price from file: " + e.getMessage());
        }
    }
    public static void sellCarToSR(Scanner input){
        addCarToShowroom(input);
    }
    public static void feedback_reader(Scanner input){

        File g = new File("feedback.txt");

        if (g.exists()){
            try {
                Scanner reader = new Scanner(g);

                while(reader.hasNextLine()){
                    String line = reader.nextLine();
                    System.out.println(line);
                    System.out.println();
                }
                reader.close();
            } catch (IOException io){
                System.out.println("File not found" + io.getMessage());
            } catch (Exception e){
                System.out.println("The error is " +e.getMessage());
            }


        }

    }
    public static void feedbackW(String feedback) {
        File file = new File("feedback.txt");

        try (BufferedWriter writer = new BufferedWriter(new FileWriter(file, true))) {
            writer.write(feedback);
            writer.newLine();
            System.out.println("Feedback recorded successfully.");
        } catch (IOException e) {
            System.out.println("An error occurred while writing to the file: " + e.getMessage());
        }
    }

}
