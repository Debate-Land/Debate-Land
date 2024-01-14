# Debate Land by NSD Methodology
Debate Land by NSD is a one-stop shop for debate analytics and statistics. We are the only platform that uses a non-elo rankings system made specifically for a subjective activity like forensics. We feature three types of statistics: computed, derived, and raw. Computed statistics use our subjective or predictive rankings formula, meaning they are built of imperfect models and are our best predictions. Whereas derived and raw statistics represent incontrovertible facts about the results of competitors. If you’re looking to improve, look at our derived and raw statistics.

## Debate Land Disclaimer
Our rankings are merely a statistical representation of results, not skill. One should not interpret their or any other debater’s stats or ranking as an analysis of skill. Debate Land serves to democratize complex or hidden information on [Tabroom.com](http://tabroom.com/) and serves as a database for future studies and self-improvement. Statistics are not answers. Just a tool.

>So, why do you have a predictive leaderboard?

In essence, our predictive leaderboard serves as a mechanism for model training and performance evaluation. By utilizing the predicted outcomes, we can make necessary adjustments to improve the accuracy of our models. This initiative to make the tool publicly accessible represents our commitment to promoting transparency and inclusivity in the debate community. It is important to note that the purpose of this tool is not to discourage tournament participation but rather to offer a valuable resource for improving the quality of information available to the debate community. Our overarching goal is to democratize the statistics of the high school debate community, and our belief is that predictive leaderboards accomplish that.

##  The OTR (3.0) Score

Debate Land uses a proprietary metric of ranking debaters colloquially known as the OTR Score. It awards points based on scaled tournament attendance, high performance (deep elimination round runs), and success against difficult opponents. Every system has flaws, but the OTR 3.0 Score is currently the only scaled, open-source metric widely available to debaters.

### **Changes from 1.0 and 2.0**

After analyzing data and user feedback from tournaments.tech and Debate Land OTR 1.0/2.0, we uncovered recurring complaints. The common denominator was that the formula disproportionately disadvantaged debaters who could not attend tournaments frequently. Additionally, our scope was limited to PF debaters on the national circuit, limiting access to thousands of debaters. We have implemented the following changes to remediate these inequalities.

1.  A linear deflator for the number of tournaments you attend. Simply put, going to more tournaments will no longer induce a disproportionate increase in score. This deflator was implemented to reduce the bias toward debaters at large schools or those who can afford to attend tournaments frequently.

2.  A round-by-round score. The V4 scraper can view round-by-round results and compile metrics based on the data. We like to call it the underdog modifier. They receive a boost if a team can beat a higher-rated team. The boost is relative to the difference in win percentages in a logarithmic regression model. This composite is tournament-by-tournament-based and incorporates an ELO model into our holistic metric. This RxR composite was introduced to reflect teams that show significant improvement.

3.  Local circuit insights. Along with additions to the formula itself, Debate Land has created the ability to add your local circuit onto the site. This includes tournament insights, competitor data, and judge data. This allows more debaters, especially those without the funding or ability to compete on the national circuit, to view their stats.

4.  Four debate types. Debate Land now includes PF, Policy, Parliamentary, and LD rankings. The new debate types were added to allow more debaters to gain insight into their statistics.

### The OTR Composite Formula (Tournament x Tournament)

$$ OTR_{comp}=(pWP*opWPm)+(\sum RxR_{comp})*(BreakBoost)*(rnpScore)$$

---
**OTR composite:** $OTR_{comp}$
This is your OTR composite score on a tournament-by-tournament basis. Higher is better; local circuit comps will be lower than large or national circuit tournaments. This formula doesn't change by circuit. 

**Prelim Win Percentage:** $pWP$
Your prelim win percentage is calculated by $\frac{numPrelimWins}{numPrelimRounds}$. PwP is derived by dividing the number of preliminary round wins by the total preliminary rounds in a tournament. It rewards teams that win more rounds in prelims. This number is always a decimal ranging from 0 to 1.00. Therefore, it serves as a deflator. It helps to validate the opWPM score.

*Implication:*  Facing outstanding teams in prelims but losing to them won’t increase your score solely because your opWPM was high; you need to win.

**Opponent Win Percentage Metric:** $opWPm$
Your opponent's win percentage metric is the average of ${\frac{opNumPrelimWins}{numPrelimRounds}}$. This metric is often generated by tabroom. It represents the average number of prelim wins your opponents had. Essentially, the summation of your opponent’s prelim wins is divided by the number of prelim opponents. This, more than anything, indicates what prelim bracket you ended in. All teams in the 3-win bracket will have a similar opWPM (which can be slightly different due to pre-matched rounds).

*Implication:* Take two teams, Team A and Team B. If team a faced two one-win teams in their pre-matched rounds, they would receive a lesser score than team b, who faced two five-win teams in their pre-matched rounds.

**Round-by-Round Composite:** $RxR_{comp}$ 
The round-by-round comp is one of the ELO-like elements of the OTR formula: 
$$RxR_{comp}=1+(\frac{x^5}{1+\sqrt{x^9}})$$ $$where$$ $$x=opPWP-PWP$$

The underdog modifier. Teams receive a boost if a team can **_beat_** a relatively higher-rated team. The boost is relative to the difference in prelim win percentages calculated by logarithmic regression. Losing to a team with a higher WP will not decrease your score. 

> **_Note:_**  _If x is negative, then a composite of 0 is assigned._

*Implication:*  A one-win team beats a six-win team in round two. Although the one-win team’s tournament comp will likely be low, having a great round will give them a slight boost and be reflected, unlike V1/2.

*Addressing possible source of error:*  Often, debaters will claim that they received a “screw” decision. For example, if a six-win team loses to a one-win team, “it was likely due to judge error.” Using the new judge data that Debate Land will gather during the 2023 season across all three supported debate formats, we will be able to analyze and determine which rounds were most likely screws. We may change the formula in future years based on our study. Until then, we ask debaters to recognize; firstly, screws are, unfortunately, a part of any sport or activity. Secondly, a judge’s decision is “law.” Unless a judge is making a decision in bad faith, they are convinced of a winner on one side. Often, there were paths to the ballot for both teams, even if not recognized. We hope debaters understand that, like a judge, our formula is imperfect but strives to come to the most accurate conclusions.

**Break Boost:** $BreakBoost$
BreakBoost supplies credit to teams who get to and win in elim rounds. It is calculated by the round you last won plus one. It is one of the largest sources of a team’s OTR comp. Teams who get to the latest out rounds will likely receive the highest tournament comps.

*Implication:*  In a tournament where the first elim round is the octafinals, a team that reaches the quarterfinals but loses will receive a break boost of two, doubling their score.

## **Final OTR Score Calculation**

$$\frac{\sum OTRcomp * avgNumTournamentsAttended}{tournamentsAttended}$$

The OTR composites are summed together and multiplied by a deflator. Simply put, going to more tournaments will no longer induce a disproportionate increase in score. This deflator was implemented to reduce the bias toward debaters at large schools or those who can afford to attend tournaments frequently.

The constants are placeholders to make the number smaller, between 0 and 5. Constants will change at the beginning and end of the season to normalize. Some years are “harder” than others (more bids among different teams). This is part of our retroactive normalization procedure on June 15th each year.

## Team Stats

### OTR 
Derived Statistic. This is the composite score that is the ultimate determinant of your rank. See above for methodology.

### BIDS

Raw Statistic. This is the number of bids (National Circuit TOC) a team has accrued. 0.5 bids indicate a silver bid for PF.

### T.RC
Raw Statistic. This is your total record across prelim and elim rounds. W-L.

### P.RC
Raw Statistic. This is your total record across prelim rounds. W-L.
### E.RC
Raw Statistic. This is your total record across elim rounds. W-L.
### PWP
Computed Statistic. This is $\frac{numPrelimWins}{numPrelimRounds}$ and increases as you win more prelim rounds. This number is displayed as a percentage. 
### TWP
Derived Statistic. This is a composite of your prelim and elim win percentages. The formula is $\frac{numPrelimWins}{numPrelimRounds}+\frac{numElimWins}{numElimRounds}*0.1$. It gives you a boost for doing well during elims at a tournament. If you never competed at elims, your $TWP$ will be the same as your $PWP$. This number is displayed as a percentage. 

### OPWPM
Computed Statistic. This is the average of your opponent's prelim win percentages, increasing as your opponents win more prelim rounds. This number is displayed as a percentage. This only changes based on "pre-matched" (non-power-matched) rounds. If you hit two good teams in your pre-matched, this will be higher. Generally, this will positively correlate with your prelim win percentage. 
### XWP
Derived Statistic. We trained a random forest machine learning model that took the input of over 100,000 rounds across PF, LD, and CX to correlate $\Delta OTRcomp$ and $PWP$ and output a win percentage. For more information on our Spark Engine, see [here](debate.land/spark) . 
### WAR
Computed Statistic. This is the difference between the rounds we expected you to win and the rounds you won. A negative $WAR$ means you generally performed worse than we expected, and a positive $WAR$ means you outperformed your OTR composite. This is an integer value.  
### WPAR
Computed Statistic. This is the difference between the win percentage of rounds we expected you to win and the percentage of rounds you won. A negative $WPAR$ means you generally performed worse than we expected, and a positive $WPAR$ means you outperformed your OTR composite. This is an integer value.  
### SPKS

Raw Statistic. This is the average speaker points you earned.
### PTS
Raw Statistic. This is the total speaker points you earned.
### -1HL
Raw Statistic. This is the average speaker points you earned, removing your highest and lowest score. 
### -2HL
Raw Statistic. This is the average speaker points you earned, removing your two highest and lowest scores. 

### σ SPKS
Computed Statistic. The standard deviation of your speaker's points from round to round. Simply put, your score is how much your speaks change on average (Consistency). Higher scores mean greater variation in speaker points (ex. 28, 29.8, 26.5, 27.5). Lower scores mean less variation in speaker points (ex. 29.8, 29.9, 28.9, 30.0). The number is a decimal.

### sigmaC.SPKS
Computed Statistic. The standard deviation of your speaker's points between partners from round to round. Simply put, your score is how much you and your partner's speaks change on average (Consistency). Higher scores mean greater variation in speaker points (ex. 28, 29.8, 26.5, 27.5). Lower scores mean less variation in speaker points (ex. 29.8, 29.9, 28.9, 30.0). Not included for LD. The number is a decimal.
### D.P.SPKS
Computed Statistic. This is the average difference in speaker points between you and your partner on a round-to-round basis. High numbers mean you and your partner often score more different speaks from one another. 
### SAA
Computed Statistic. This is an average between your speaker points and the average speaker points of everybody else at that tournament. A larger positive number indicates you spoke higher relative to the pool. 
### SAJ
Computed Statistic. This is an average between your speaker points and the average speaker points of everybody else your judges scored at that tournament. A larger positive number indicates you spoke higher relative to the pool your judges scored. 
### OSI
Computed Statistic. Opponent Intimidation Index. This measures, if your opponents had higher or lower, speaks compared to their average when they faced you. A positive number means your opponents speak better when they face you on the net. 
### LAY
Derived Statistic. Your record when you have lay judges. We identify judges as lay/flow with our Spark Neural Engine. For more information on our Spark Engine, see [here](debate.land/spark) . 
### LAYWP
Derived Statistic. Your win percentage when you have lay judges. We identify judges as lay/flow with our Spark Neural Engine. For more information on our Spark Engine, see [here](debate.land/spark) . 
### FLO
Derived Statistic. Your record when you have flow judges. We identify judges as lay/flow with our Spark Neural Engine. For more information on our Spark Engine, see [here](debate.land/spark) . 
### FLOWP
Derived Statistic. Your win percentage when you have flow judges. We identify judges as lay/flow with our Spark Neural Engine. For more information on our Spark Engine, see [here](debate.land/spark) . 
### HPR
Raw Statistic. Your record when you have higher speaks than your opponents. 
### HPWP
Computed Statistic. Your win percentage when you have higher speaks than your opponents. 
### LPR
Raw Statistic. Your record when you have higher speaks than your opponents. 
### LPWP
Computed Statistic. Your win percentage when you have higher speaks than your opponents. 
### UDR
Derived Statistic. Your record when you have a win percentage below 50%. We derive $XWP$ with our Spark Neural Engine. For more information on our Spark Engine, see [here](debate.land/spark)
### UDRWP
Derived Statistic. Your win percentage when you have a win percentage below 50%. We derive $XWP$ with our Spark Neural Engine. For more information on our Spark Engine, see [here](debate.land/spark)
### FVR 
Derived Statistic. Your record when you have a win percentage above 50%. We derive $XWP$ with our Spark Neural Engine. For more information on our Spark Engine, see [here](debate.land/spark)
### FVRWP
Derived Statistic. Your win percentage when you have a win percentage above 50%. We derive $XWP$ with our Spark Neural Engine. For more information on our Spark Engine, see [here](debate.land/spark)


