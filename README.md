# Restaurant-management-system
import java.util.ArrayList;
import java.util.Scanner;

public class RestaurantManagementSystem {
    public static void main(String[] args) {
        Restaurant restaurant = new Restaurant("My Restaurant");

        // Add some initial menu items
        restaurant.addMenuItem(new MenuItem("Burger", 5.99));
        restaurant.addMenuItem(new MenuItem("Pizza", 8.99));
        restaurant.addMenuItem(new MenuItem("Salad", 4.99));

        // Add some initial tables
        restaurant.addTable(new Table(1));
        restaurant.addTable(new Table(2));
        restaurant.addTable(new Table(3));

        // Display menu and manage orders
        Scanner scanner = new Scanner(System.in);
        while (true) {
            System.out.println("\nWelcome to " + restaurant.getName());
            System.out.println("1. Display Menu");
            System.out.println("2. Place Order");
            System.out.println("3. View Orders");
            System.out.println("4. Exit");
            System.out.print("Choose an option: ");
            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (choice) {
                case 1:
                    restaurant.displayMenu();
                    break;
                case 2:
                    restaurant.placeOrder(scanner);
                    break;
                case 3:
                    restaurant.viewOrders();
                    break;
                case 4:
                    System.out.println("Thank you for using the Restaurant Management System. Goodbye!");
                    scanner.close();
                    return;
                default:
                    System.out.println("Invalid option. Please try again.");
            }
        }
    }
}

class Restaurant {
    private String name;
    private ArrayList<MenuItem> menu;
    private ArrayList<Table> tables;

    public Restaurant(String name) {
        this.name = name;
        this.menu = new ArrayList<>();
        this.tables = new ArrayList<>();
    }

    public String getName() {
        return name;
    }

    public void addMenuItem(MenuItem item) {
        menu.add(item);
    }

    public void addTable(Table table) {
        tables.add(table);
    }

    public void displayMenu() {
        System.out.println("\nMenu:");
        for (MenuItem item : menu) {
            System.out.println(item);
        }
    }

    public void placeOrder(Scanner scanner) {
        System.out.print("Enter table number: ");
        int tableNumber = scanner.nextInt();
        scanner.nextLine(); // Consume newline

        Table table = getTableByNumber(tableNumber);
        if (table == null) {
            System.out.println("Invalid table number.");
            return;
        }

        System.out.println("Enter menu item name to order (type 'done' to finish):");
        while (true) {
            String itemName = scanner.nextLine();
            if (itemName.equalsIgnoreCase("done")) {
                break;
            }

            MenuItem item = getMenuItemByName(itemName);
            if (item != null) {
                table.addOrder(new Order(item));
                System.out.println("Added " + item.getName() + " to the order.");
            } else {
                System.out.println("Menu item not found.");
            }
        }
    }

    public void viewOrders() {
        System.out.println("\nOrders:");
        for (Table table : tables) {
            table.displayOrders();
        }
    }

    private MenuItem getMenuItemByName(String name) {
        for (MenuItem item : menu) {
            if (item.getName().equalsIgnoreCase(name)) {
                return item;
            }
        }
        return null;
    }

    private Table getTableByNumber(int number) {
        for (Table table : tables) {
            if (table.getNumber() == number) {
                return table;
            }
        }
        return null;
    }
}

class MenuItem {
    private String name;
    private double price;

    public MenuItem(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public double getPrice() {
        return price;
    }

    @Override
    public String toString() {
        return name + " - $" + price;
    }
}

class Table {
    private int number;
    private ArrayList<Order> orders;

    public Table(int number) {
        this.number = number;
        this.orders = new ArrayList<>();
    }

    public int getNumber() {
        return number;
    }

    public void addOrder(Order order) {
        orders.add(order);
    }

    public void displayOrders() {
        System.out.println("Table " + number + " Orders:");
        for (Order order : orders) {
            System.out.println(order);
        }
    }
}

class Order {
    private MenuItem menuItem;

    public Order(MenuItem menuItem) {
        this.menuItem = menuItem;
    }

    public MenuItem getMenuItem() {
        return menuItem;
    }

    @Override
    public String toString() {
        return menuItem.getName() + " - $" + menuItem.getPrice();
    }
}
