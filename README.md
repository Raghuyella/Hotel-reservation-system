# Hotel-reservation-system
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

// Room class
class Room {
    private String type;
    private double price;
    private boolean available;

    public Room(String type, double price) {
        this.type = type;
        this.price = price;
        this.available = true;
    }

    public String getType() {
        return type;
    }

    public double getPrice() {
        return price;
    }

    public boolean isAvailable() {
        return available;
    }

    public void setAvailable(boolean available) {
        this.available = available;
    }
}

// Reservation class
class Reservation {
    private Room room;
    private String customerName;

    public Reservation(Room room, String customerName) {
        this.room = room;
        this.customerName = customerName;
    }

    public Room getRoom() {
        return room;
    }

    public String getCustomerName() {
        return customerName;
    }

    @Override
    public String toString() {
        return "Reservation{" +
                "roomType='" + room.getType() + '\'' +
                ", customerName='" + customerName + '\'' +
                '}';
    }
}

// Hotel class
class Hotel {
    private List<Room> rooms = new ArrayList<>();
    private List<Reservation> reservations = new ArrayList<>();

    public void addRoom(Room room) {
        rooms.add(room);
    }

    public List<Room> searchAvailableRooms() {
        List<Room> availableRooms = new ArrayList<>();
        for (Room room : rooms) {
            if (room.isAvailable()) {
                availableRooms.add(room);
            }
        }
        return availableRooms;
    }

    public boolean makeReservation(String customerName, Room room) {
        if (room.isAvailable()) {
            room.setAvailable(false);
            reservations.add(new Reservation(room, customerName));
            return true;
        }
        return false;
    }

    public List<Reservation> getReservations() {
        return reservations;
    }
}

// Payment class
class Payment {
    public static boolean processPayment(double amount) {
        // Simulate payment processing
        System.out.printf("Processing payment of $%.2f... Payment successful!%n", amount);
        return true; // In a real application, you'd check the payment gateway response.
    }
}

// Main class
public class HotelReservationSystem {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Hotel hotel = new Hotel();

        // Adding some rooms to the hotel
        hotel.addRoom(new Room("Single", 100));
        hotel.addRoom(new Room("Double", 150));
        hotel.addRoom(new Room("Suite", 250));

        while (true) {
            System.out.println("\n1. Search Available Rooms");
            System.out.println("2. Make a Reservation");
            System.out.println("3. View Reservations");
            System.out.println("4. Exit");
            System.out.print("Choose an option: ");
            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (choice) {
                case 1:
                    List<Room> availableRooms = hotel.searchAvailableRooms();
                    if (availableRooms.isEmpty()) {
                        System.out.println("No rooms available.");
                    } else {
                        System.out.println("Available Rooms:");
                        for (Room room : availableRooms

) {
                            System.out.printf("Type: %s, Price: $%.2f%n", room.getType(), room.getPrice());
                        }
                    }
                    break;

                case 2:
                    System.out.print("Enter your name: ");
                    String name = scanner.nextLine();
                    List<Room> roomsToReserve = hotel.searchAvailableRooms();
                    if (roomsToReserve.isEmpty()) {
                        System.out.println("No rooms available for reservation.");
                    } else {
                        System.out.println("Available Rooms:");
                        for (int i = 0; i < roomsToReserve.size(); i++) {
                            Room room = roomsToReserve.get(i);
                            System.out.printf("%d. Type: %s, Price: $%.2f%n", i + 1, room.getType(), room.getPrice());
                        }
                        System.out.print("Choose a room number to reserve: ");
                        int roomChoice = scanner.nextInt() - 1;

                        if (roomChoice >= 0 && roomChoice < roomsToReserve.size()) {
                            Room selectedRoom = roomsToReserve.get(roomChoice);
                            if (Payment.processPayment(selectedRoom.getPrice())) {
                                hotel.makeReservation(name, selectedRoom);
                                System.out.println("Reservation made successfully!");
                            }
                        } else {
                            System.out.println("Invalid room selection.");
                        }
                    }
                    break;

                case 3:
                    List<Reservation> reservations = hotel.getReservations();
                    if (reservations.isEmpty()) {
                        System.out.println("No reservations found.");
                    } else {
                        System.out.println("Reservations:");
                        for (Reservation reservation : reservations) {
                            System.out.println(reservation);
                        }
                    }
                    break;

                case 4:
                    System.out.println("Exiting the system.");
                    scanner.close();
                    return;

                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }
}
