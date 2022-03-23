 # 战斗结构设计
   Battle() => Init() ===>
        ||    ||        ||
        ||      Replay()=>
        ||              ||   
        ||              ||
        ||              GetInitData();//获取战斗初始化数据
        ||              ||
        ||              BattleType()=> 
        ||              ||          ||
        ||              ||          Base/World =>
        ||              ||                       SimplifyBattle()==>
        ||              ||                                          ||=>  InitUnitDisplay()=> LoadUnit();
        ||              WholeBattle()===============================>
        ||
        Update()=>BattlePlay()//每一帧更新扎战斗动画
                           ||
                
        战斗过程=====>> 
        PrepareBattle();
          ||
        Turn Start()
          ||
        Frame=<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<Frame>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
          ||                                                            
        IsDead()=======> false => PreAttack()==> CheckHurt()=> IsDead()========
        ||                              ||      YES
        ||                          IsSkill?  =====> CheckState()=> CanTakeTurn()=>  sKill()
        ||                              ||
        ||                              || False
        ||                              ||
        ||                          Attack()=> 
        ||
        || Yes
        ||
        SkipTrun()