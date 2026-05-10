Refer to the assignment spec on myBeckett
Class name - Myrailway.java 
package MyRailWay;
import uk.ac.leedsbeckett.oop.*;

@SuppressWarnings("serial")
public class Myrailway extends OOPrailwaySim {

    // Track train state
    private boolean trainStopped = false;

    // Count rounds
    private int rounds = 0;

    public Myrailway() {

        super();

        setVisible(true);

        System.out.println("===== RAILWAY CONTROL SYSTEM =====");
        System.out.println("Type Commands:");
        System.out.println("--------------------------------");
        System.out.println("about");
        System.out.println("addloco <x> <y>");
        System.out.println("addslowloco <x> <y>");
        System.out.println("addcar <locoId>");
        System.out.println("detachcarriage <locoId>");
        System.out.println("start");
        System.out.println("stop");
        System.out.println("speed <number>");
        System.out.println("cross <number>");
        System.out.println("reset");
        System.out.println("green   -> Train moves");
        System.out.println("red     -> Train stops");
        System.out.println("danger  -> Emergency stop");
        System.out.println("rounds  -> Show completed rounds");
        System.out.println("--------------------------------");
    }

    // Called automatically when user types in GUI text field
    @Override
    public void processCommand(String input) {

        System.out.println("Command Entered: " + input);

        try {

            // ABOUT
            if (input.equalsIgnoreCase("about")) {
                about();
            }

            // ADD NORMAL LOCO
            else if(input.startsWith("addloco ")) {

                String[] parts = input.split(" ");

                if(parts.length == 3) {

                    int x = Integer.parseInt(parts[1]);
                    int y = Integer.parseInt(parts[2]);

                    int loco = addLocomotive(new Locomotive(world, x, y));

                    displayOutput("Locomotive added. ID = " + loco);
                }
                else {
                    displayOutput("Usage: addloco <x> <y>");
                }
            }

            // ADD SLOW LOCO
            else if(input.startsWith("addslowloco ")) {

                String[] parts = input.split(" ");

                if(parts.length == 3) {

                    int x = Integer.parseInt(parts[1]);
                    int y = Integer.parseInt(parts[2]);

                    NewLoco newLoco = new NewLoco(world, x, y);

                    int id = addLocomotive(newLoco);

                    newLoco.setLocoId(id);

                    displayOutput("Slow Locomotive added. ID = " + id);
                }
                else {
                    displayOutput("Usage: addslowloco <x> <y>");
                }
            }

            // ADD CARRIAGE
            else if(input.startsWith("addcar ")) {

                String[] parts = input.split(" ");

                if(parts.length == 2) {

                    int locoId = Integer.parseInt(parts[1]);

                    addCarriageToLocomotive(locoId, new Carriage(world));

                    displayOutput("Carriage added to locomotive : " + locoId);
                }
                else {
                    displayOutput("Usage: addcar <locoId>");
                }
            }

            // DETACH CARRIAGE
            else if(input.startsWith("detachcarriage ")) {

                String[] parts = input.split(" ");

                if(parts.length == 2) {

                    int locoId = Integer.parseInt(parts[1]);

                    deleteCarriage(locoId);

                    displayOutput("Carriage detached from locomotive : " + locoId);
                }
                else {
                    displayOutput("Usage: detachcarriage <locoId>");
                }
            }

            // START SIMULATION
            else if(input.equalsIgnoreCase("start")) {

                startSimulation();

                displayOutput("Simulation Started.");
            }

            // STOP SIMULATION
            else if(input.equalsIgnoreCase("stop")) {

                stopSimulation();

                displayOutput("Simulation Stopped.");
            }

            // SPEED CONTROL
            else if(input.startsWith("speed ")) {

                String[] parts = input.split(" ");

                if(parts.length == 2) {

                    int speed = Integer.parseInt(parts[1]);

                    startSimulation(speed);

                    displayOutput("Simulation speed set to : " + speed);
                }
                else {
                    displayOutput("Usage: speed <number>");
                }
            }

            // TOGGLE CROSSING
            else if(input.startsWith("cross ")) {

                String[] parts = input.split(" ");

                if(parts.length == 2) {

                    int cross = Integer.parseInt(parts[1]);

                    toggleCrossing(cross);

                    displayOutput("Crossing toggled : " + cross);
                }
                else {
                    displayOutput("Usage: cross <crossingNumber>");
                }
            }

            // RESET
            else if(input.equalsIgnoreCase("reset")) {

                resetSimulation();

                rounds = 0;

                displayOutput("Simulation Reset.");
            }

            // GREEN SIGNAL
            else if(input.equalsIgnoreCase("green")) {

                trainStopped = false;

                startSimulation();

                displayOutput("GREEN SIGNAL -> TRAIN MOVING");
            }

            // RED SIGNAL
            else if(input.equalsIgnoreCase("red")) {

                trainStopped = true;

                stopSimulation();

                displayOutput("RED SIGNAL -> TRAIN STOPPED");
            }

            // DANGER SIGNAL
            else if(input.equalsIgnoreCase("danger")) {

                trainStopped = true;

                stopSimulation();

                displayOutput("DANGER SIGNAL -> EMERGENCY STOP");
            }

            // ROUND COUNTER
            else if(input.equalsIgnoreCase("rounds")) {

                displayOutput("Rounds Completed : " + rounds);
            }

            // HELP COMMAND
            else if(input.equalsIgnoreCase("help")) {

                displayOutput(
                    "Commands:\n" +
                    "about\n" +
                    "addloco <x> <y>\n" +
                    "addslowloco <x> <y>\n" +
                    "addcar <locoId>\n" +
                    "detachcarriage <locoId>\n" +
                    "start\n" +
                    "stop\n" +
                    "speed <number>\n" +
                    "cross <number>\n" +
                    "reset\n" +
                    "green\n" +
                    "red\n" +
                    "danger\n" +
                    "rounds"
                );
            }

            // INVALID COMMAND
            else {

                displayOutput("Invalid Command : " + input);
            }

        }

        catch(NumberFormatException e) {

            displayOutput("Invalid number entered.");
        }

        catch(GameWorldException e) {

            displayOutput("Locomotive cannot be placed here. No track found.");
        }

        catch(Exception e) {

            displayOutput("Error : " + e.getMessage());
        }
    }

    @Override
    public void about() {

        super.about();

        displayOutput(
            "Railway Simulation System\n" +
            "Assignment By : Jyothasana Sharma\n" +
            "Student ID : c7686074"
        );
    }
}


class name - MainClass.java
package MyRailWay;
import java.util.Scanner;

public class MainClass {

    public static void main(String[] args) {

        Myrailway railway = new Myrailway();

        // Create locomotive
        NewLoco loco = new NewLoco(railway.getWorld(), 4, 3);

        Scanner scan = new Scanner(System.in);

        System.out.println("===== RAILWAY CONTROL SYSTEM =====");
        System.out.println("Commands:");
        System.out.println("about");
        System.out.println("green  -> Train moves");
        System.out.println("red    -> Train stops");
        System.out.println("danger -> Emergency stop");
        System.out.println("exit");

        while (scan.hasNextLine()) {

            String input = scan.nextLine();

            if (input.equalsIgnoreCase("about")) {
                railway.about();
            }

            else if (input.equalsIgnoreCase("green")) {
                loco.startTrain();
            }

            else if (input.equalsIgnoreCase("red")) {
                loco.stopTrain("RED");
            }

            else if (input.equalsIgnoreCase("danger")) {
                loco.stopTrain("DANGER");
            }

            else if (input.equalsIgnoreCase("exit")) {
                System.out.println("System Closed.");
                break;
            }

            else {
                System.out.println("Invalid Command : " + input);
            }
        }

        scan.close();
    }
}


Class name - NewLoco.java
package MyRailWay;
import uk.ac.leedsbeckett.oop.Locomotive;
import uk.ac.leedsbeckett.oop.GameWorld;

public class NewLoco extends Locomotive {

    public int locoId = -1;

    // Traffic Light States
    private boolean stopped = false;

    // Count completed rounds
    private int rounds = 0;

    // Store previous position
    private String lastPosition = "";

    public NewLoco(GameWorld world, int x, int y) {
        super(world, x, y);
        setAnimationStepPixels(2);
    }

    public void setLocoId(int id) {
        this.locoId = id;
    }
    public int getRounds() {
        return rounds;
    }

    // RED or DANGER = STOP
    public void stopTrain(String reason) {
        stopped = true;
        System.out.println(reason + " SIGNAL -> TRAIN STOPPED");
    }

    // GREEN = MOVE
    public void startTrain() {
        stopped = false;
        System.out.println("GREEN SIGNAL -> TRAIN MOVING");
    }

    @Override
    public String toString() {
        return "Train Position : " + getCellPosition() +
               " | Rounds Completed : " + rounds;
    }

    @Override
    public void tick() {

        // Train only moves if not stopped
        if (!stopped) {
            super.tick();
        }

        String currentPosition = getCellPosition().toString();

        // Detect one full round
        if (!lastPosition.equals("") &&
            currentPosition.equals("java.awt.Point[x=1,y=1]")) {

            rounds++;
            System.out.println("ROUND COMPLETED -> " + rounds);
        }

        lastPosition = currentPosition;

        System.out.println(this.toString());
    }
}




There is useful information in the assignment spec directory on myBeckett, including help videos
