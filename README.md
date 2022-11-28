# Credit Default Swap Option Valuation


The European credit default swap option (CDSO) valuation model is employed to price an option that grants its holder the right, but not the obligation, to enter into a Credit Default Swap (CDS) at some future point in time. The premium to be paid on this forward-start CDS is fixed in advance at some strike level. If the reference entity should default before the forward-start date, the contract is in null and no payments are made.

A closed form solution, which is similar to the Black’s model for interest rate swaption, is implemented within the current credit library framework. The forward credit spread of the underlying CDS is computed using pricing model for CDS and the counterparty risk is handled with default correlation model. Apart from pricing, the model can be employed to back out the implied volatility of the forward credit spread.


The European Credit Default Swap Option (CDSO) pricing model serves the purpose of pricing an option that grants its holder the right, but not the obligation, to enter into a Credit Default Swap (CDS) at some future point in time. The premium to be paid on this forward-start CDS is fixed in advance at some strike level. If the reference entity should default before the forward-start date, the contract is in null and no payments are made.

It is natural to try to value a CDSO by using an approach similar to that of the valuing a European swaption in interest rate markets (see https://finpricing.com/lib/FxAccumulator.html). It has been proved that a modified Black future pricing closed form solution can be derived to price CDSO, by which the model follows.

In the following of the report, we assume the valuation date t, the forward-start date of CDS   (maturity of CDSO), and the forward CDS fixed fee payments at interval   (maturity of CDS). 

The underlying asset of the CDSO is the forward-start CDS.  In order to calculate the forward CDS credit spread, we need first to determine the default probability of the underlying reference entity.  

Within the current credit derivatives framework a reduced form approach, which can be viewed as the current industry standard of modelling the default probability of an entity, has been implemented and well maintained. Let the default time of the CDS reference entity be  , a deterministic risk-neutral hazard rate,  , is defined such that the default probability between s and s+ds  . With this definition the default probability functions built upon the hazard rate are
(1)	 = survival probability seen at time t.
(2)	  = default probability density function seen at time t

There are two valuation legs for the forward-start CDS: fee and protection. If we assume a risk free counterparty, the risky annuity of the fee leg is calculated by:
 (3)  .
Payment for the fee during the period   is made at   if default has not occurred;   is the day count fraction; D(t,s) is the discount factor; the notional is set to be 1.
The value of the protection leg can be expressed as 
(4)	 ,
where R is the recovery rate and   is maturity of the contract.

The forward CDS spread  , which is the fixed premium paid by the CDS buyer, can then be determined by
(5)	 .

The payoff function of CDSO at maturity can be written as

(6)	 ,

where  takes the value +1(-1) for an option on a CDS where the option holder pays(receives) the fixed premium. K is the strike spread of the CDSO at maturity. The price of CDSO seen at time t has the form:

(7)	 .

Notice that

(8)	 

is strictly positive and we could introduce another probability measure  ,  under which Eq. (7) can be rewritten as

(9)	 .

It has been proved that under  , which is sometimes called “survival measure”,   is a martingale. Similar to the European swaption, we can assume a lognormal stochastic process of the forward CDS spread

(10)		 ,

where    is a Brownian motion under  . We then can get the Black-Scholes type result of the CDSO price at time t 

(11)	 ,

where N(.) is the cumulative normal distribution and 

(12)	 

It is important to notice that the CDSO will be in null, if the reference name defaults before the maturity of the option. Sometimes people would be interested in buying protection before the maturity the option along with the CDSO. The fair value of this upfront payment, called knockout value, is the value of protection of the underlying CDS from the settlement date of the option until the start date of the underlying, which reads

(13)	 .

Two risk measures are implemented in the submitted model: credit spread sensitivity and volatility sensitivity.  The credit spread sensitivity is the change of value when the credit spread on each node of the credit spread curve is perturbed. The credit spread curve, which is employed to calculate implied default probability of the reference name, is a set of credit spreads of CDS with different maturity. Then the ith credit spread sensitivity is calculated by perturbing the ith CDS spread 1 basis point up

 (14)	 

for all i belongs to the credit spread curve.

The volatility sensitivity, known as Vega, is a measure of the option price sensitivity to a change in the volatility. 

(15)	 .

If the counterparty of the CDSO has the right to enter the forward starting CDS to sell protection ( ) , the counterparty risk should be considered. Within the credit derivatives modelling framework, the counterparty is described by its own hazard rate curve and corresponding recovery rate. The default correlation between the counterparty and the reference name of the forward CDS is inferred from their asset correlation. Following the same modelling procedure of the CDS with counterparty risk, we could have the adjusted forward CDS spread 

(16)     ,


where   is the hazard rate for the event that the reference obligor always defaults before the counterparty.    and   are recovery rates for the reference name and counterparty, respectively.

Once we have the forward CDS spread, we could use same Black style model to price the CDSO, which is exactly the same as Eq.(11).

Just like the Black model in the interest rate market, the volatility of the forward credit spread is not a market observable. Hence one purpose of the CDSO model is to provide a method to back out implied volatility information from the market. 

When we price the CDOS using this model, we would expect that the volatility could be implied from the market. If there is no liquid option market for a given name, the volatility information could be found by proxy to other names. If the implied volatility cannot be found from the market, a historical analysis, as proposed by Hull and White1, would be used to find a reasonable estimation.

The model for valuing European CDSO, as outlined in this report, is very similar to the standard market model of for valuing European swap option. The main assumption of the model is that the forward CDS spread follows a lognormal stochastic process. Given the fact that this approach has been quite successful in the interest rate market, we could expect that this CDSO model can quickly become a market standard1. This standard model enables market participants to calculate an implied volatility from a price or a price from volatility, which is essential for an active options market. In fact, this model has also been implemented in GAT. 

For the CDSO model with counterparty risk, we employ Monte Carlo simulation as a comparison model to calculate the adjusted forward spread and then using Black’s model to compute the option value. 
