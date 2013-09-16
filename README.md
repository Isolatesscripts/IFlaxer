IFlaxer
=======
import java.awt.Graphics;
import java.awt.Point;

import org.shadowbot.bot.api.methods.data.Bank;
import org.shadowbot.bot.api.methods.data.Inventory;
import org.shadowbot.bot.api.methods.input.Mouse;
import org.shadowbot.bot.api.methods.interactive.GroundItems;
import org.shadowbot.bot.api.methods.interactive.Locatable;
import org.shadowbot.bot.api.methods.interactive.Players;
import org.shadowbot.bot.api.movement.Walking;
import org.shadowbot.bot.api.util.Time;
import org.shadowbot.bot.api.wrapper.GroundItem;
import org.shadowbot.bot.api.wrapper.Manifest;
import org.shadowbot.bot.api.wrapper.Tile;
import org.shadowbot.bot.api.wrapper.listeners.PaintListener;
import org.shadowbot.bot.client.debug.script.Script;
/**
 * Created with IntelliJ IDEA.
 * User: Jordan
 * Date: 16/09/13
 * Time: 8:00 AM
 * To change this template use File | Settings | File Templates.
 */
public class IFlaxer extends Script implements PaintListener {
    @Manifest(scriptname = "IFlaxer",description = "Picks Dem Flaxz", author = "Isolate", category = "Moneymaking")
                  final Tile AtBank{}
                  final Tile BankToFlax{}
                  final Tile FlaxToBank{}
                  final Tile BankTile{}

    @Override
    public int loop(){
        GroundItem Flax = GroundItems.getNearest("Flax");
        if (Inventory.isFull()) {
            if (AtBank()) {
                BankStuff();
            } else {
                WalkToBank();
            }
        } else {
            PickFlax();

        }else{
            WalkToFlax();
        }

    }
    private void WalkToBank(){
        Walking.walkTo(FlaxToBank);
        for (int i = 0; i < 10 && Players.getLocal().isMoving(); i++, Time
                .sleep(200, 250))
            ;

    }
   private void BankStuff(){
       Bank.open();
       BankAll();
       Time.sleep(500, 600);
   }
    private void BankAll(){
        Mouse.click(new Point(444, 311), true);
        Mouse.click(true);
    }
   private void PickFlax(){
       GroundItem Flax = GroundItems.getNearest("Flax");
       if (Flax.isOnScreen()) {
           Flax.interact("Pick");
           for (int i = 0; i < 10 && Players.getLocal().isMoving(); i++, Time
                   .sleep(200, 250))
               ;
       }else{
           Walking.walkTo(Flax);
           for (int i = 0; i < 10 && Players.getLocal().isMoving(); i++, Time
                   .sleep(200, 275))
               ;
       }
   }
   private void WalkToFlax(){

       Walking.walkTo(BankToFlax);
       for (int i = 0; i < 10 && Players.getLocal().isMoving(); i++, Time
               .sleep(200, 250))
           ;
   }
    public boolean AtBank() {
        return (((Locatable) BankTile).distanceTo() < 4);
    }


    @Override
    public int onStop(){

    }

}
