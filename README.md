# HISGOT-COFFEE-RECEIPT
package com.mycompany.mavenproject2;

import java.util.Scanner;

public class HisgotCoffeeReceipt {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        String[] menuItems = {
                // ☕ Coffee (1–12)
                "Espresso", "Cappuccino", "Latte", "Americano", "Mocha",
                "Caramel Macchiato", "Vanilla Latte", "Iced Coffee",
                "Cold Brew", "Spanish Latte", "Hazelnut Latte", "Matcha Latte",

                // 🍟 Snacks (13–22)
                "Blueberry Muffin", "Chocolate Cake Slice", "Croissant",
                "Cheesecake Slice", "Club Sandwich", "French Fries",
                "Garlic Bread", "Donut", "Pancakes", "Waffles",

                // 🍰 Desserts (23–28)
                "Chocolate Sundae", "Strawberry Sundae", "Halo-Halo",
                "Leche Flan", "Brownies", "Ice Cream Cone"
        };

        double[] prices = {
                // Coffee prices
                99, 120, 130, 110, 140,
                150, 145, 100, 135, 155, 145, 160,

                // Snack prices
                85, 120, 95, 130, 160, 90,
                80, 70, 110, 115,

                // Dessert prices
                120, 120, 150,
                90, 85, 60
        };

        boolean continueCustomer = true;

        while (continueCustomer) {
            double total = 0;
            int[] quantities = new int[menuItems.length]; // ✅ store orders

            System.out.println("\n===== HISGOT COFFEE SHOP =====");

            displayMenu(menuItems, prices);

            // ORDER LOOP
            while (true) {
                System.out.print("\nEnter item number (1-28) or 0 to finish: ");
                int choice = scanner.nextInt();

                if (choice == 0) break;

                if (choice >= 1 && choice <= menuItems.length) {
                    System.out.print("Enter quantity for " + menuItems[choice - 1] + ": ");
                    int qty = scanner.nextInt();

                    if (qty > 0) {
                        quantities[choice - 1] += qty; // ✅ store quantity
                        double itemTotal = qty * prices[choice - 1];
                        total += itemTotal;

                        System.out.println(menuItems[choice - 1] + " x" + qty + " added.");
                    } else {
                        System.out.println("Invalid quantity!");
                    }
                } else {
                    System.out.println("Invalid item number!");
                }
            }

            // APPLY DISCOUNT
            total = applyDiscount(total, scanner);

            // VAT
            double vat = total * 0.12;
            double finalTotal = total + vat;

            // RECEIPT
            System.out.println("\n========== RECEIPT ==========");

            System.out.println("\nItems Purchased:");
            for (int i = 0; i < menuItems.length; i++) {
                if (quantities[i] > 0) {
                    double itemTotal = quantities[i] * prices[i];
                    System.out.printf("%-20s x%d - PHP %.2f%n",
                            menuItems[i], quantities[i], itemTotal);
                }
            }

            System.out.printf("\nSubtotal: PHP %.2f%n", total);
            System.out.printf("VAT (12%%): PHP %.2f%n", vat);
            System.out.printf("TOTAL: PHP %.2f%n", finalTotal);

            // PAYMENT LOOP (FIXED ✅)
            double payment = 0;
            while (payment < finalTotal) {
                System.out.print("Enter payment: PHP ");
                payment = scanner.nextDouble();

                if (payment < finalTotal) {
                    System.out.printf("Insufficient payment! Need PHP %.2f more.%n",
                            (finalTotal - payment));
                }
            }

            // CHANGE
            System.out.printf("CHANGE: PHP %.2f%n", (payment - finalTotal));
            System.out.println("Thank you!");

            // NEXT CUSTOMER
            System.out.print("\nNew customer (yes/no): ");
            scanner.nextLine(); // clear buffer
            String again = scanner.nextLine();

            if (!again.equalsIgnoreCase("yes")) {
                continueCustomer = false;
            }
        }

        scanner.close();
    }

    // DISPLAY MENU
    private static void displayMenu(String[] menuItems, double[] prices) {
        System.out.println("\n--- COFFEE ---");
        for (int i = 0; i < 12; i++) {
            System.out.printf("%2d. %-20s - PHP %.2f%n",
                    (i + 1), menuItems[i], prices[i]);
        }

        System.out.println("\n--- SNACKS ---");
        for (int i = 12; i < 22; i++) {
            System.out.printf("%2d. %-20s - PHP %.2f%n",
                    (i + 1), menuItems[i], prices[i]);
        }

        System.out.println("\n--- DESSERTS ---");
        for (int i = 22; i < menuItems.length; i++) {
            System.out.printf("%2d. %-20s - PHP %.2f%n"
