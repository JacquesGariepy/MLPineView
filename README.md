# MLPineScript

//@version=5
strategy('Pine Script Strategy 2', overlay=true, shorttitle='PSTES2', initial_capital=1000, default_qty_value=100, default_qty_type=strategy.percent_of_equity, commission_value=0.025)
fastPeriod = input(title='Fast MA', defval=24)
slowPeriod = input(title='Slow MA', defval=200)
fastEMA = ta.ema(close, fastPeriod)
fastSMA = ta.sma(close, fastPeriod)
slowEMA = ta.ema(close, slowPeriod)
atr = ta.atr(14)
goLongCondition1 = fastEMA > fastSMA
goLongCondition2 = fastEMA > slowEMA
exitCondition1 = fastEMA < fastSMA
exitCondition2 = close < slowEMA
inTrade = strategy.position_size > 0
notInTrade = strategy.position_size <= 0
timePeriod = time >= timestamp(syminfo.timezone, 2020, 12, 15, 0, 0)
if timePeriod and goLongCondition1 and goLongCondition2 and notInTrade
    strategy.entry('long', strategy.long, when=notInTrade)
    stopLoss = close - atr * 3
    strategy.exit('exit', 'long', stop=stopLoss)
if exitCondition1 and exitCondition2 and inTrade
    strategy.close(id='long')
plot(fastEMA, color=color.new(color.blue, 0))
plot(slowEMA, color=color.new(color.yellow, 0))
bgcolor(notInTrade ? color.red : color.green, transp=90)
