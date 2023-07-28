//Name: Michael Wang
//Date: January 28 2021
//CPT: Allyway player combat game
package gamewithmethods;


import java.util.Random; //import random number generator
import java.util.Scanner; //import scanner
import java.util.concurrent.TimeUnit; //import time delay

public class Gamewithmethods {
    public static final String ANSI_BLACK_BACKGROUND = "\u001B[40m";  //color text
	public static final String ANSI_RED_BACKGROUND = "\u001B[41m";
	public static final String ANSI_GREEN_BACKGROUND = "\u001B[42m";
	public static final String ANSI_YELLOW_BACKGROUND = "\u001B[43m";
	public static final String ANSI_BLUE_BACKGROUND = "\u001B[44m";
	public static final String ANSI_PURPLE_BACKGROUND = "\u001B[45m";
	public static final String ANSI_CYAN_BACKGROUND = "\u001B[46m";
	public static final String ANSI_WHITE_BACKGROUND = "\u001B[47m";
	public static final String ANSI_RESET = "\u001B[0m";
	public static final String ANSI_BLACK = "\u001B[30m";
	public static final String ANSI_RED = "\u001B[31m";
	public static final String ANSI_GREEN = "\u001B[32m";
	public static final String ANSI_YELLOW = "\u001B[33m";
	public static final String ANSI_BLUE = "\u001B[34m";
	public static final String ANSI_PURPLE = "\u001B[35m";
	public static final String ANSI_CYAN = "\u001B[36m";
	public static final String ANSI_WHITE = "\u001B[37m";
	static double wins=0;
        
    public static final String [] enemies = {"Old Henchman", "Sneaky Goblin", "Feisty Mongoose", 
    		"Unhinged Zebra", "Untamed Bull", "Possessed dragon"}; //array that randomly generates an enemy for the user to fight in level 1

	static double winpercent=0;
        static int option=0;
        static int input=0;
        static Scanner in = new Scanner (System.in);
	public static void main(String[] args)throws InterruptedException { //main method
	    String playAgain = "";
	    do{ //do while loop that goes to the intro method, while play again does not equal to no
	      intro();
	      playAgain = in.nextLine();
	    }while(!playAgain.equalsIgnoreCase("no")); 
	  }
	
	public static void endGame(int signal, String msg) { //end game method that exits out of the loop
		if (signal<1) {
			System.out.println(msg);
			System.exit(0);
		}
	}
	
	public static void intro()throws InterruptedException{ //method that introduces the game to the user
      try { //error proofing
          System.out.println(ANSI_BLUE+"Welcome to the Creeky alleyway..... \n");
	    System.out.println("\n" + ANSI_YELLOW_BACKGROUND+"Enter 3 to see the rules for level 1 and level 2");
	    System.out.println(ANSI_CYAN+"Enter 4 to jump directly to level 1");
        System.out.println(ANSI_RED_BACKGROUND+"Enter 5 to jump directly to level 2");  //introduction to the game
        input=in.nextInt();
      }
      catch (Exception e){ //error proofing
          System.out.println(ANSI_GREEN+"Invalid input. Please enter 3 to see the rules, 4 to jump to the game, and 5 to jump to level 2.");
      }
	    switch(input){ //switch statement where the methods are located in each class
	      case 3:
	        rules();
	        break;
	      case 4:
	        level1();
	      case 5:
	        level2();
	    }
  }
  public static void rules()throws InterruptedException{ //method that shows the user the rules of the game
      System.out.println("FOR LEVEL 1.....");
                System.out.println("You enter a secluded alleyway. Before you engage with the enemies, YOU must enter 1 to attack, 2 to drink a health potion");
                System.out.println("Or 3 to run away from the enemy, and face off with a random enemy...");
                System.out.println("----------------");
                System.out.println("FOR LEVEL 2.....");
                System.out.println("This level is much more challenging, as YOU MUST CHOOSE 2 ENEMIES TO FIGHT WITH. This time, your");
                System.out.println("choice of running away is stripped from you. SO YOU MUST BE BRAVE YOUNG ONE.....");
               
  }
  public static void level1()throws InterruptedException{ //method that allows the user to start the game
	  Random rand=new Random();
        ///text adventure game variables
        int maxenemyhealth = 100;
        int enemyattackdamage = 35;
        
        //boolean variable 
        boolean cont=false; 
        
        //Player Variables
        int userhealth=100;
        int attackdamage=50;
        int numhealthpotions=3;
        int healthpotionhealamount=30;
        int healthpotiondropchance=50;   //50% chance to drop a health potion(perentage)
        
        boolean run=true;
        
        STARTGAME: //starts game, while run is true
        while (run){
            System.out.println("-----------------------------------");
            int enemyhealth=rand.nextInt(maxenemyhealth);
            String enemy = enemies[rand.nextInt(enemies.length)];
            System.out.println(ANSI_CYAN+"\t#"+enemy+" has appeared!! #\n");
            //while the enemyhealth and userhealth is greater than zero, the game will continue
            while (enemyhealth>0 && userhealth>0){    
            	
            	preGame1(enemy, userhealth, enemyhealth); //The program will jump down into the preGame1 method
            	
            	Scanner in2 = new Scanner (System.in); //new scanner for preGame1
            	String option=in2.nextLine(); //new option variable for preGame1
                             
                //decision statement for attack
                if (option.equals("1")){
                    System.out.println("\nBe ready to suffer either a fateful blow, or a weak punch....");
                    TimeUnit.SECONDS.sleep(2);
                    System.out.println("The enemy is attacking now....");
                    TimeUnit.SECONDS.sleep(2);
                    int damageinflicted=rand.nextInt(attackdamage); //random amount of health taken away from the enemy
                    int damagetaken=rand.nextInt(enemyattackdamage); //random amount of health taken away from the user
                    enemyhealth-=damageinflicted; //accumulator
                    userhealth-=damagetaken; //accumulator
                    
                    System.out.println("\t> It looks like you have hit "+enemy+" for "+damageinflicted+" damage point(s)!!");
                    System.out.println(enemy+" has hit you for "+damagetaken+" HP!\n");
                    TimeUnit.SECONDS.sleep(2); //time delay
                    //program counts to when the user health is -1, that way it's more accurate
                    if (userhealth<1){
                        endGame(-1, "\t> You have taken too much damage. You are too weak to continue.....");
                    }
                    int nextGame=0; //new variable nextGame
                    if (enemyhealth<=0){                        
                        nextGame = getNextGame(nextGame, 1, enemy, null); //new method for nextGame
                    }
                    while (nextGame==1){ //while nextGame is 1(when the user enters yes to play again), the game will continue to relapse
                    	continue STARTGAME;
                    }
                    if (nextGame==2){ //if the user doesn't want to play again, then the program will exit out of the loop, and stop.
                        endGame(-1,ANSI_BLUE+ "Thank you for playing my dearest friend!!! I hope to someday see you again.....");
                    }
                }
                else if (option.equals("2")){
                    //option 2 for when the user takes a potion
                    if (numhealthpotions>0){
                        userhealth+=healthpotionhealamount; //counter for how much the user healed him/herself
                        numhealthpotions--; //takes away the potions left. 
                        System.out.println("Opening up elixer bottle...");
                        TimeUnit.SECONDS.sleep(2);
                        System.out.println("Rejuvenating your energy....");
                        TimeUnit.SECONDS.sleep(2);
                        System.out.println("\t> You drank a health potion, to heal yourself up for "+healthpotionhealamount+" HP!");
                        System.out.println("You now have "+userhealth+" HP left");
                        System.out.println("You now have "+numhealthpotions+" potion(s) left.");
                    }
                    else {
                        System.out.println("\t> You have no health potions left. No easy way out!!! You must defeat an enemy in order to gain a health potion!!!: ");
                    }
                }
                else if (option.equals("3")){
                    //running away option for the user
                    System.out.println("\t> You ran away from "+enemy+"!");
                    continue STARTGAME;
                }
            }
            System.out.println("-----------------------------------");
            System.out.println("#"+enemy+" was defeated by you");
            System.out.println(" # You have " +userhealth+" HP left.");
            if (rand.nextInt(100) < healthpotiondropchance){
                numhealthpotions++;
                System.out.println("The enemy dropped a health potion!");
                System.out.println("You now have "+numhealthpotions+" health potion(s).");
            }
            endGame(-1, "-+-+-+-+-+-+-+-+-+-+-+THANK YOU FOR PLAYING. "
        		+ "HOPE IT GAVE YOU A TASTE OF WHATS OFFERED INSIDE THE DUNGEON!"
        		+ "-+-+-+-+-+-+-+-+-+-+-+");
     }  
  }
  public static int getNextGame(int localnextGame, int level, String enemy, String [] enemies)throws InterruptedException { 
      //getnextGame method that acts as a transitioning period from asking the user if they want to play again or not, to moving on to level 2
	  if (level ==1)
		  System.out.println("You have defeated "+enemy+".");
	  else if (level==2) 
		  System.out.println("You have defeated "+enemies[0]+" and " + enemies[1] + ".");
          
      System.out.println("");
      System.out.println(ANSI_BLUE+"------------------------------");
      System.out.println("");
      System.out.println(ANSI_RED_BACKGROUND+"Do you want to play again?: Enter 1 for yes and 2 for no");
	
	  int returnnextGame = getInput(localnextGame, 1, 2);
      
      if (level==2 && returnnextGame==1) {
    	  System.out.println("....................updating enemy list");
    	  String [] newenemies = pickLevel2Enemies();
    	  enemies[0] = newenemies[0];
    	  enemies[1] = newenemies[1];	  
      }
      return returnnextGame;
  }
  public static void preGame1(String enemya, int userhealtha, int enemyhealtha)throws InterruptedException { 
      //preGame1 method describes the new choices that the user can choose from
      //this is excluding the run away option, to increase difficulty
      //It shows the user and enemys health
      System.out.println("\tYour HP: "+userhealtha);
      System.out.println("\t"+enemya+"'s HP: "+enemyhealtha);
      System.out.println("");
      System.out.println("-+-+-+-+-+-+-+-+HERE ARE YOUR CHOICES-+-+-+-+-+-+-+-+");
      System.out.println("\t What would you like to do?: ");
      System.out.println("\t1. Attack");
      System.out.println("\t2. Drink health potion to rejuvenate yourself");
      System.out.println("\t3. Run away from the enemy and fight another enemy");
  }
  public static void preGame2(String [] enemya, int userhealtha, int [] enemyhealtha) {
      //the preGame2 method shows both enemies health so far
      System.out.println("\tYour HP: "+userhealtha);
      System.out.println("\t"+enemya[0]+"'s HP: "+enemyhealtha[0]); //enemy 1
      System.out.println("\t"+enemya[1]+"'s HP: "+enemyhealtha[1]); //enemy 2
      System.out.println("");
      System.out.println("-+-+-+-+-+-+-+-+HERE ARE YOUR CHOICES-+-+-+-+-+-+-+-+");
      System.out.println("\t Unfortunately you can't run away anymore. What would you like to do? ");
      System.out.println("\t1. Attack");
      System.out.println("\t2. Drink health potion to rejuvenate yourself");
  }

  public static int getInput(int localnext, int low, int high) { 
      //getInput method coutns for when the user enters an invalid command to the program. 
	Scanner scannerNextGame = new Scanner(System.in);
	  int returnnext = localnext;
      while (returnnext == 0) {
      	String nextLine;
          try {
          	nextLine=scannerNextGame.nextLine();
          	returnnext = Integer.parseInt(nextLine);
          	if (returnnext < low || returnnext >high) {
          		System.out.println("Invalid option. Please try again.");
          		returnnext=0;
          		continue;
          	}
          } catch(Exception e) {
          	System.out.println("Invalid option. Please try again.");
          	continue;
          }
      }
      return returnnext; //returns returnnext back into the main method, to try and get a valid user input from the user
                        //which is the main purpose of this particular method
  }
  public static String [] pickLevel2Enemies() {
      //pickLevelEnemies method allows the user to pick their two enemies from the arrays list provided for them
      System.out.println("Not challenging enough? You will now be able to fight two enemies head on....");
      	  Scanner in2 = new Scanner(System.in);
	  
	  String enemyLevel2List [] = new String[2];
	  
	  System.out.println("Please choose your first enemy:");
	  for (int i=0; i < enemies.length;i++) { //traversing an array
		  System.out.println((i+1) + " - " + enemies[i]);
	  }
	  
	  
	  int iOption2 =getInput(0, 1, 6);
	  
	  System.out.println(" your 1st enemy: " + iOption2); //first enemy choice
	  enemyLevel2List[0] = enemies[iOption2-1];
	  
	  System.out.println("Please choose your second enemy:");
	 
	  for (int i=0; i < enemies.length;i++) { //traversing an array
		  if (! enemies[i].equalsIgnoreCase( enemyLevel2List[0].toString()))
		  System.out.println((i+1) + " - " + enemies[i]); 
	  }
	  iOption2 =in2.nextInt();
	  System.out.println("Your 2nd enemy: " + iOption2); //second enemy choice
	  enemyLevel2List[1] = enemies[iOption2-1];
	  
	  return enemyLevel2List;
  }
  
  public static boolean areEnemiesHealthy(int [] health)throws InterruptedException {
	  if (health[0] > 0 && health[1] >0)
		  return true; //this method areEnemiesHealthy will return true, if both enemies chosen by the user in level 2 are above zero
	  return false; //else, it will return false
  }
  
   public static void level2()throws InterruptedException{ //method for level 2, that now allows the user to fight against the two enemies they chose in the pickLevel2Enemies method
     
      // present a list enemies to choose from
	   String [] enemyLevel2List = pickLevel2Enemies();
		  System.out.println("Here is your enemy list. Are you ready?");
		  for (int i=0; i < enemyLevel2List.length;i++) { //traversing the array
			  System.out.println((i+1) + " - " + enemyLevel2List[i]);
		  }
	  // Now you just need to fight them
		  Random rand=new Random();
	        ///text adventure game variables
	        int maxenemyhealth = 100;
	        int enemyattackdamage = 35;
	        
	        boolean cont=false;
	        
	        //Player Variables
	        int userhealth=200;
	        int attackdamage=50;
	        int numhealthpotions=4;
	        int healthpotionhealamount=40;
	        
	        boolean run=true; //boolean variable run declared
	        
	        STARTGAME:
	        while (run){ //while run is true, the program will function smooothly
	            System.out.println("-----------------------------------");
	            int [] enemyhealth= {rand.nextInt(maxenemyhealth), rand.nextInt(maxenemyhealth)}; //random enemy health generated
	            
	            while (areEnemiesHealthy(enemyhealth) && userhealth>0){ //while this comdition is met, it will jump back into the preGame2 method
	            	
	            	preGame2(enemyLevel2List, userhealth, enemyhealth);

                        Scanner in2 = new Scanner (System.in);
	            	String option=in2.nextLine();
	                             
	                if (option.equals("1")){
	                    System.out.println("\nBe ready to suffer either a fateful blow, or a weak punch....");
	                    //TimeUnit.SECONDS.sleep(2);
	                    System.out.println("The enemies are attacking now....");
	                    //TimeUnit.SECONDS.sleep(2);
	                    int [] damageinflicted= {rand.nextInt(attackdamage), rand.nextInt(attackdamage)}; 
	                    int damagetaken=rand.nextInt(enemyattackdamage) + rand.nextInt(enemyattackdamage) ;
	                    enemyhealth[0] -= damageinflicted[0];
	                    enemyhealth[1] -= damageinflicted[1];
	                    userhealth-=damagetaken;
	                    
	                    System.out.println("\t> It looks like you have hit "+enemyLevel2List[0]+" for "+damageinflicted[0]+" damage point(s)!!"); //hitting the first enemy for a certain amount of damage points
	                    System.out.println("\t> It looks like you have hit "+enemyLevel2List[1]+" for "+damageinflicted[1]+" damage point(s)!!");//hitting the first enemy for a certain amount of damage points
	                    System.out.println(enemyLevel2List[0] + " and " + enemyLevel2List[1]+" have hit you for "+damagetaken+" HP!\n");
	                    //TimeUnit.SECONDS.sleep(2);
	                    if (userhealth<1){
	                        endGame(-1, "\t> You have taken too much damage. You are too weak to continue....."); //if the user health is lower than zero, the program will stop
	                    }
	                    int nextGame=0;
	                    // Option for next game if any enemy is neutralized
	                    if (! areEnemiesHealthy(enemyhealth) ){                        
	                        nextGame = getNextGame(nextGame, 2, null, enemyLevel2List);
	                    }
	                    while (nextGame==1){ //if the user enters 1 for yes, then they will play the game again
	                    	continue STARTGAME;
	                    }
	                    if (nextGame==2){ //if they enter 2 for no, then the program will stop
	                        endGame(-1, "Thank you for playing.... We look forward to seeing you next time sir/ma'am....");
                                System.out.println(ANSI_RED+"---------I hope you enjoyed being in the creepy dark damp alleyway facing all of these obscure enemies...---------");
                                System.out.println(ANSI_RED+"----------Now that you've passed this, you'll be an unstoppable force in the real world....-----------");
	                    }
	                }
	                else if (option.equals("2")){
                            //drinking option for level 2 for the user
	                    if (numhealthpotions>0){
	                        userhealth+=healthpotionhealamount;
	                        numhealthpotions--;
	                        System.out.println("Opening up elixer bottle...");
	                        TimeUnit.SECONDS.sleep(2);
	                        System.out.println("Rejuvenating your energy....");
	                        TimeUnit.SECONDS.sleep(2);
	                        System.out.println("\t> You drank a health potion, to heal yourself up for "+healthpotionhealamount+" HP!");
	                        System.out.println("You now have "+userhealth+" HP left");
	                        System.out.println("You now have "+numhealthpotions+" potion(s) left.");
	                    }
	                    else {
	                        System.out.println("\t> You have no health potions left. No easy way out!!! You must defeat an enemy in order to gain a health potion!!!: ");
	                    }
	                }
	            }
	            System.out.println("-----------------------------------");
	            System.out.println("#"+enemyLevel2List[0] + " and " + enemyLevel2List[1]+" were defeated by you");
	            System.out.println(" # You still have " +userhealth+" HP left.");
	            
	            
	            endGame(-1, "-+-+-+-+-+-+-+-+-+-+-+THANK YOU FOR PLAYING. "
	        		+ "HOPE IT GAVE YOU A TASTE OF WHATS OFFERED INSIDE THE ALLYWAY!"
	        		+ "-+-+-+-+-+-+-+-+-+-+-+");
	     } 
   }
}
