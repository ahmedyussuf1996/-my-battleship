-my-battleship
==============

av ahmed yussuf te 12c

package  
{
	import flash.display.Bitmap;
	import flash.display.Sprite;
	/**
	 * ...
	 * @author Ahmed
	 */
	public class Tile extends Sprite
	{
		[Embed(source = "Vatten1.png")]
		private var vattenBild:Class;
		
		[Embed(source = "VattenHit.png")]
		private var vattenTräff:Class;
		
		[Embed(source = "VattenMiss.png")]
		private var vattenMiss:Class;
		
		
		public static const WATER:int = 1;
		public static const WATERHIT:int = 2;
		public static const WATERMISS:int = 3;
		public var thisShip:Boolean = false;
		public var BattleShip:int = 3;
		
		public static const TILE_SIDE:int = 48;
		private var typeOfTerrain:int = 0;
		
		public var scoreOrNot:Boolean;
		
		
		
		public function Tile() 
		{
			
			this.graphics.beginBitmapFill(Bitmap(new vattenBild()).bitmapData);
			this.graphics.drawRect(0, 0, TILE_SIDE, TILE_SIDE);
			this.graphics.endFill();
		}
		
		public function clicked():void 
		{
		
			if (this.thisShip == true) 
			{
				this.thisShip = BattleShip;
				this.graphics.clear();
				this.graphics.beginBitmapFill(Bitmap(new vattenTräff()).bitmapData);
				this.graphics.drawRect(0, 0, TILE_SIDE, TILE_SIDE);
				this.graphics.endFill();
				scoreOrNot = true;
				
				if (scoreOrNot = true) 
				{
					getScore();
				}
				
				
			}
			else 
			{
				this.graphics.clear();
				this.graphics.beginBitmapFill(Bitmap(new vattenMiss()).bitmapData);
				this.graphics.drawRect(0, 0, TILE_SIDE, TILE_SIDE);
				this.graphics.endFill();
			}
			
			
		}
		
		public function getScore():int 
		{
			return + 1;
		}
		
		
		
	}

}


det här är min tile




package 
{
	import flash.display.SpreadMethod;
	import flash.display.Sprite;
	import flash.events.Event;
	import flash.events.KeyboardEvent;
	import flash.events.MouseEvent;
	import flash.text.TextField;
	
	/**
	 * ...
	 * @author Ahmed
	 */
	public class Main extends Sprite 
	{
		public var karta:Vector.<Vector.<Tile>> = new Vector.<Vector.<Tile>>();
		private var scoreText:TextField;
		private var score:Number;
		
		
		
		public function Main():void 
		{
			if (stage) init();
			else addEventListener(Event.ADDED_TO_STAGE, init);
		}
		
		private function init(e:Event = null):void 
		{
			removeEventListener(Event.ADDED_TO_STAGE, init);
			// entry point
			
			var pressSpace:TextField = new TextField();
			pressSpace.text = "Press Space";
			pressSpace.x = 350;
			pressSpace.y = 250;
			addChild(pressSpace);
			
			
			
			stage.addEventListener(KeyboardEvent.KEY_DOWN, keyDown);
		}
		private function newMap():void 
		{
			score = 0;
			scoreText = new TextField;
			scoreText.x = 50;
			addChild(scoreText);
			updateScore();
			
			for (var i:int = 0; i < 10; i++) 
			{
				var enRad:Vector.<Tile> = new Vector.<Tile>();
				for (var j:int = 0; j < 10; j++) 
				{
					var t:Tile = new Tile();
					t.y = 100 + j * (Tile.TILE_SIDE + 1);
					t.x = 100 + i * (Tile.TILE_SIDE + 1);
					this.addChild(t);
					this.addEventListener(MouseEvent.CLICK, clickTheBox);
					enRad.push(t);
					
				}
				karta.push(enRad);
				
 			}
		}
		
		private function keyDown(e:KeyboardEvent):void 
		{
			if (e.keyCode == 32) 
			{	
				//Reset allt på kartan
				
				while (numChildren > 0) 
				{
					removeChildAt(0);
				}
				// töm vectorn
				karta.length = 0;
				//Skapar nytt spel
				newMap();
				//Scoren resetas
				score = 0;
				scoreText.text = "Score: 0"
				
				PlaceShip(3);
				PlaceShip(3);
				PlaceShip(3);
				
	
			}
		}
		private function clickTheBox(e:MouseEvent):void 
		{
			Tile(e.target).clicked();
			
			if (Tile(e.target).scoreOrNot) 
			{
				score++;
				updateScore();
			}
		}
		private function updateScore():void 
		{
			scoreText.text = "Score: " + score;
		}
		private function PlaceShip(shipLength:int = 3):void
		{
			var isHorizontal:Boolean = false;
			var shipX:int;
			var shipY:int;
			var currentShip:Vector.<Tile> = new Vector.<Tile>();
			if (Math.random() > 0.5)
			{
				isHorizontal = true;
			}
			if (isHorizontal)
			{
				shipX = Math.random() * (karta.length-shipLength);
				shipY = Math.random() * (karta.length - 1);
				for (var i:int = 0; i < shipLength; i++) 
				{
					currentShip.push(karta[shipX+i][shipY]);
				}
			}
			else
			{
				shipX = Math.random() * (karta.length - 1);
				shipY = Math.random() * (karta.length-shipLength);
				for (var i:int = 0; i < shipLength; i++) 
				{
 					currentShip.push(karta[shipX][shipY+i]);
				}
			}
			for (var j:int = 0; j < currentShip.length; j++) 
			{
				if (currentShip[j].thisShip)
				{
					PlaceShip(shipLength);
					return;
				}
			}
			for (var k:int = 0; k < currentShip.length; k++) 
			{
				currentShip[k].thisShip = true;
			}
		}
	}
	
}


min main 
