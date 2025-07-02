import java.time.LocalTime;
import java.util.*;

public class GoalReminder {

    static class Goal {
        String task;
        LocalTime time;

        Goal(String task, LocalTime time) {
            this.task = task;
            this.time = time;
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        List<Goal> goals = new ArrayList<>();
        Timer timer = new Timer();

        System.out.println("Welcome to the Daily Goal Reminder!");
        System.out.print("How many goals would you like to set today? ");
        int count = getValidInteger(scanner);

        for (int i = 0; i < count; i++) {
            System.out.print("\nEnter goal #" + (i + 1) + ": ");
            String task = scanner.nextLine();

            System.out.print("Enter reminder time for \"" + task + "\" in HH:mm format (24-hour): ");
            LocalTime time = getValidTime(scanner);

            goals.add(new Goal(task, time));

            // Schedule reminder
            long delay = getDelayInMillis(time);
            if (delay > 0) {
                timer.schedule(new TimerTask() {
                    @Override
                    public void run() {
                        System.out.println("🔔 Reminder: " + task);
                    }
                }, delay);
            } else {
                System.out.println("⚠️ Skipped scheduling for \"" + task + "\". Time already passed.");
            }
        }

        System.out.println("\n✅ All goals scheduled. The application will remind you accordingly.");
    }

    // Helper methods
    private static LocalTime getValidTime(Scanner scanner) {
        while (true) {
            try {
                String input = scanner.nextLine();
                return LocalTime.parse(input);
            } catch (Exception e) {
                System.out.print("45 ");
            }
        }
    }

    private static int getValidInteger(Scanner scanner) {
        while (true) {
            try {
                return Integer.parseInt(scanner.nextLine());
            } catch (NumberFormatException e) {
                System.out.print("45 ");
            }
        }
    }

    private static long getDelayInMillis(LocalTime targetTime) {
        LocalTime now = LocalTime.now();
        long delaySeconds = java.time.Duration.between(now, targetTime).getSeconds();
        return delaySeconds > 0 ? delaySeconds * 1000 : -1;
    }
}
