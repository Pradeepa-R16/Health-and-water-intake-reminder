import java.awt.*;
import java.util.Timer;
import java.util.TimerTask;

public class HealthReminder {

    // Function to show desktop notification
    public static void showNotification(String title, String message) {
        // Check if SystemTray is supported
        if (SystemTray.isSupported()) {
            SystemTray tray = SystemTray.getSystemTray();
            Image image = Toolkit.getDefaultToolkit().createImage("icon.png");

            TrayIcon trayIcon = new TrayIcon(image, "Health Reminder");
            trayIcon.setImageAutoSize(true);
            trayIcon.setToolTip("Health & Water Intake Reminder");

            try {
                tray.add(trayIcon);
                trayIcon.displayMessage(title, message, TrayIcon.MessageType.INFO);
                tray.remove(trayIcon); // Remove after showing
            } catch (Exception e) {
                System.out.println("TrayIcon could not be added: " + e.getMessage());
            }
        } else {
            System.out.println("System tray not supported!");
        }
    }

    public static void main(String[] args) {
        Timer timer = new Timer();

        // Reminder every 1 minute (60000 ms) -> you can change to 30*60*1000 for 30 mins
        int interval = 60 * 1000;

        timer.schedule(new TimerTask() {
            int count = 0;

            @Override
            public void run() {
                count++;

                if (count % 1 == 0) { // Every 1 minute
                    showNotification("💧 Water Break", "Time to drink a glass of water!");
                }
                if (count % 3 == 0) { // Every 3 minutes
                    showNotification("🧘 Stretch Break", "Stand up and stretch for a minute!");
                }
                if (count % 5 == 0) { // Every 5 minutes
                    showNotification("👀 Eye Exercise", "Look away from screen for 20 seconds!");
                }
            }
        }, 0, interval);

        System.out.println("Health Reminder started... Press Ctrl+C to stop.");
    }
}
