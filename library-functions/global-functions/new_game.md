# new\_game

## `new_game()`

Dies ist der Einstiegspunkt, der von einer Karte aufgerufen wird. Schon bevor das Spiel seinen ersten Tick durchlaufen hat, wird diese Funktion aufgerufen. Normalerweise werden in dieser Funktion Sachen festgelegt, die schon am Spielbeginn feststehen. Dies beinhaltet zum Beispiel festgelegte Objekte die platziert werden oder die Kampfkraft einer Partei. Auch werden hier Events angemeldet.

#### Rückgabewert

Da diese Funktion selbst geschrieben wird und vom Spiel intern aufgerufen wird, hat diese Funktion keinen Rückgabewert

#### Beispiel

```lua
----
-- Script von "Wacah Chan"
----
function new_game()

--//Anfang Spielereinstellungen//--

     --//Angriffsmodi//--
     AI.SetPlayerVar(2,"AttackMode",1,2,2)
     AI.SetPlayerVar(3,"AttackMode",1,2,2)
   
     --//Angriffsfolge//--
     AI.SetPlayerVar(2,"AttackDelay",50,40,90)
     AI.SetPlayerVar(3,"AttackDelay",60,30,90)

     --//Soldatenanzahl//--
     AI.SetPlayerVar(2,"SoldierLimitAbsolute",150,300,1000)
     AI.SetPlayerVar(2,"SoldierLimitRelative",75,250,700)

     AI.SetPlayerVar(3,"SoldierLimitAbsolute",150,300,1000)
     AI.SetPlayerVar(3,"SoldierLimitRelative",75,250,700)

--//Ende Spielereinstellungen//--


--registriere Funktion VictoryConditionCheck als Siegesbedingungscheck
  request_event(VictoryConditionCheck, Events.VICTORY_CONDITION_CHECK)
end

function register_functions()
--registriere VictoryConditionCheck als Funktion, die in mind. einem Event vorkommt
  reg_func(VictoryConditionCheck)
end

function VictoryConditionCheck()
  -- Prüfe ob durch Standartregeln ein Spieler verloren hat
  Game.DefaultPlayersLostCheck()
 
  -- wenn Spieler 2 und 3 Veloren haben und Spieler 1 noch im Spiel ist, verlieren alle Gegner von 1
  if Game.HasPlayerLost(1) == 0
  and Game.HasPlayerLost(2) == 1
  and Game.HasPlayerLost(3) == 1
  then
    Game.EnemyPlayersLost(1)
  end
 
  -- Notwendig, damit die Nachrichten wie "Sie haben Gewonnen" erscheinen
  Game.DefaultGameEndCheck()
endlu
```

