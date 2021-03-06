# TopEDH
Analysis of most-played cards in the Commander format of Magic the Gathering.

## About Magic the Gathering

Magic: The Gathering (MTG; also known as Magic) is a trading card game created by Richard Garfield.

First published in 1993 by Wizards of the Coast, Magic was the first trading card game produced and it continues to thrive, with approximately twenty million players as of 2015. Magic can be played by two or more players in various formats, the most common of which uses a deck of 60+ cards, either in person with printed cards or using a deck of virtual cards through the Internet-based Magic: The Gathering Online, on a smartphone or tablet, or other programs.

Each game represents a battle between wizards known as "planeswalkers", who employ spells, artifacts, and creatures depicted on individual Magic cards to defeat their opponents. Although the original concept of the game drew heavily from the motifs of traditional fantasy role-playing games such as Dungeons & Dragons, the gameplay of Magic bears little similarity to pencil-and-paper adventure games, while having substantially more cards and more complex rules than many other card games.

New cards are released on a regular basis through expansion sets. An organized tournament system played at an international level and a worldwide community of professional Magic players has developed, as well as a substantial secondary market for Magic cards. Certain Magic cards can be valuable due to their rarity and utility in gameplay. Prices range from a few cents to thousands of dollars.

(From [Wikipedia](https://en.wikipedia.org/wiki/Magic:_The_Gathering))

## About the Commander Format

Created and popularized by fans, the Commander (also known as "EDH") variant is usually played in casual Free-for-All multiplayer games, although two-player games are also popular. Each player's deck is headed by a legendary creature (a character within the game world) designated as that deck's commander. A player's choice of commander determines which other cards can be played in the deck (while except for basic lands, each card in the deck must have a different name).

(From [MTG Salvation Wiki](http://mtgsalvation.gamepedia.com/Commander_(format)))

## About TopEDH

TopEDH is a report designed to assist Commander players in purchasing decisions. By combining pricing data and decklists, we look at a "decks/dollar" metric called "Value". In other words, how many unique decks (one per commander) would a player want to put this card in, divided by how much they will pay for the card. In this way, players can optimize their purchasing to fill as many slots as possible within a budget. 

### A Note About Power Level

While the "power" of a card is difficult to quantify, one might suspect that the occurences of a card in decks indicates its gameplay value. 

Unfortunately, this is not the case. By direct observation, an expert can see that functionally similar but stronger cards sometimes appear at lower occurences. We suspect this relates to age, release count, and presence on the Reserved List, and prevelance of play in other formats. Further analysis may incorporate these factors to optimize for quality of card purchases in addition to quantity. 

# Setup


```python
%matplotlib inline
```


```python
import numpy, pandas, seaborn
```


```python
edh = pandas.read_csv("/home/aaron/src/python/topedh/output.csv", 
                      delimiter=";")
```

# Highest Occurences Overall

<b>Occurences</b>: The number of commanders among decks in which this card often sees play. By measuring a card based on which decks it can be found, we neutralize the effect of commander popularity on a card's rating in this list. Otherwise, cards played with popular commanders would be artificially inflated.

Occurences were scraped from edhrec.com using Beautiful Soup.

## Basic Stats


```python
mean = numpy.mean(edh['occurences'])
std = numpy.std(edh['occurences'])
median = numpy.median(edh['occurences'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 9.068072866730585
    Standard Deviation: 21.036265630734256
    Median: 3.0



```python
tscores = (edh['occurences'] - mean)/std
tscores = list(zip(edh['title'], 
                   edh['occurences'],
                   edh['occurences']/489,
                   tscores))

df = pandas.DataFrame(data=tscores, 
                      columns=['Title', 
                               'Commanders',
                               'VS S. Ring',
                               'Z-Score (Commanders)'])

df = df.sort_values('Z-Score (Commanders)', ascending=0)
df[:25]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Title</th>
      <th>Commanders</th>
      <th>VS S. Ring</th>
      <th>Z-Score (Commanders)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>sol ring</td>
      <td>489</td>
      <td>1.000000</td>
      <td>22.814502</td>
    </tr>
    <tr>
      <th>1</th>
      <td>temple of the false god</td>
      <td>394</td>
      <td>0.805726</td>
      <td>18.298491</td>
    </tr>
    <tr>
      <th>2</th>
      <td>lightning greaves</td>
      <td>373</td>
      <td>0.762781</td>
      <td>17.300215</td>
    </tr>
    <tr>
      <th>3</th>
      <td>swiftfoot boots</td>
      <td>365</td>
      <td>0.746421</td>
      <td>16.919920</td>
    </tr>
    <tr>
      <th>4</th>
      <td>reliquary tower</td>
      <td>349</td>
      <td>0.713701</td>
      <td>16.159329</td>
    </tr>
    <tr>
      <th>5</th>
      <td>solemn simulacrum</td>
      <td>328</td>
      <td>0.670757</td>
      <td>15.161052</td>
    </tr>
    <tr>
      <th>6</th>
      <td>evolving wilds</td>
      <td>252</td>
      <td>0.515337</td>
      <td>11.548244</td>
    </tr>
    <tr>
      <th>7</th>
      <td>command tower</td>
      <td>239</td>
      <td>0.488753</td>
      <td>10.930264</td>
    </tr>
    <tr>
      <th>8</th>
      <td>terramorphic expanse</td>
      <td>232</td>
      <td>0.474438</td>
      <td>10.597505</td>
    </tr>
    <tr>
      <th>9</th>
      <td>caged sun</td>
      <td>232</td>
      <td>0.474438</td>
      <td>10.597505</td>
    </tr>
    <tr>
      <th>10</th>
      <td>gilded lotus</td>
      <td>229</td>
      <td>0.468303</td>
      <td>10.454894</td>
    </tr>
    <tr>
      <th>11</th>
      <td>nykthos, shrine to nyx</td>
      <td>220</td>
      <td>0.449898</td>
      <td>10.027061</td>
    </tr>
    <tr>
      <th>12</th>
      <td>burnished hart</td>
      <td>187</td>
      <td>0.382413</td>
      <td>8.458342</td>
    </tr>
    <tr>
      <th>13</th>
      <td>thran dynamo</td>
      <td>186</td>
      <td>0.380368</td>
      <td>8.410805</td>
    </tr>
    <tr>
      <th>14</th>
      <td>darksteel ingot</td>
      <td>184</td>
      <td>0.376278</td>
      <td>8.315731</td>
    </tr>
    <tr>
      <th>15</th>
      <td>sensei's divining top</td>
      <td>181</td>
      <td>0.370143</td>
      <td>8.173120</td>
    </tr>
    <tr>
      <th>16</th>
      <td>expedition map</td>
      <td>177</td>
      <td>0.361963</td>
      <td>7.982972</td>
    </tr>
    <tr>
      <th>17</th>
      <td>myriad landscape</td>
      <td>177</td>
      <td>0.361963</td>
      <td>7.982972</td>
    </tr>
    <tr>
      <th>18</th>
      <td>skullclamp</td>
      <td>176</td>
      <td>0.359918</td>
      <td>7.935435</td>
    </tr>
    <tr>
      <th>19</th>
      <td>demonic tutor</td>
      <td>170</td>
      <td>0.347648</td>
      <td>7.650214</td>
    </tr>
    <tr>
      <th>20</th>
      <td>diabolic tutor</td>
      <td>166</td>
      <td>0.339468</td>
      <td>7.460066</td>
    </tr>
    <tr>
      <th>21</th>
      <td>cyclonic rift</td>
      <td>160</td>
      <td>0.327198</td>
      <td>7.174844</td>
    </tr>
    <tr>
      <th>22</th>
      <td>bojuka bog</td>
      <td>159</td>
      <td>0.325153</td>
      <td>7.127307</td>
    </tr>
    <tr>
      <th>23</th>
      <td>swords to plowshares</td>
      <td>158</td>
      <td>0.323108</td>
      <td>7.079770</td>
    </tr>
    <tr>
      <th>24</th>
      <td>commander's sphere</td>
      <td>154</td>
      <td>0.314928</td>
      <td>6.889622</td>
    </tr>
  </tbody>
</table>
</div>



# Highest Playable Value Overall

Here we define <b>playable value</b> to be occurences divided by price. Or, in other words "decks per dollar". Price has been adjusted from the mid value found at dawnglare.com. Cards without a listed price are assumed to be $0.50, which could be wildly incorrect. These cards are relatively cheap and played in many decks. Price Data was accessed on 6/5/2016.


```python
d = {'Title': edh['title'], 
     'Value': edh['value'], 
     'Type': edh['type']}
df = pandas.DataFrame(data=d)

df = df.sort_values('Value', ascending=0)
df[:25]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Title</th>
      <th>Type</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>temple of the false god</td>
      <td>Land</td>
      <td>788.00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>swiftfoot boots</td>
      <td>Artifact — Equipment</td>
      <td>730.00</td>
    </tr>
    <tr>
      <th>6</th>
      <td>evolving wilds</td>
      <td>Land</td>
      <td>504.00</td>
    </tr>
    <tr>
      <th>8</th>
      <td>terramorphic expanse</td>
      <td>Land</td>
      <td>464.00</td>
    </tr>
    <tr>
      <th>12</th>
      <td>burnished hart</td>
      <td>Artifact Creature — Elk</td>
      <td>374.00</td>
    </tr>
    <tr>
      <th>14</th>
      <td>darksteel ingot</td>
      <td>Artifact</td>
      <td>368.00</td>
    </tr>
    <tr>
      <th>20</th>
      <td>diabolic tutor</td>
      <td>Sorcery</td>
      <td>332.00</td>
    </tr>
    <tr>
      <th>24</th>
      <td>commander's sphere</td>
      <td>Artifact</td>
      <td>308.00</td>
    </tr>
    <tr>
      <th>29</th>
      <td>mind stone</td>
      <td>Artifact</td>
      <td>298.00</td>
    </tr>
    <tr>
      <th>37</th>
      <td>rogue's passage</td>
      <td>Land</td>
      <td>284.00</td>
    </tr>
    <tr>
      <th>49</th>
      <td>acidic slime</td>
      <td>Creature — Ooze</td>
      <td>238.00</td>
    </tr>
    <tr>
      <th>59</th>
      <td>steel hellkite</td>
      <td>Artifact Creature — Dragon</td>
      <td>202.00</td>
    </tr>
    <tr>
      <th>0</th>
      <td>sol ring</td>
      <td>Artifact</td>
      <td>196.39</td>
    </tr>
    <tr>
      <th>63</th>
      <td>everflowing chalice</td>
      <td>Artifact</td>
      <td>196.00</td>
    </tr>
    <tr>
      <th>22</th>
      <td>bojuka bog</td>
      <td>Land</td>
      <td>184.88</td>
    </tr>
    <tr>
      <th>70</th>
      <td>halimar depths</td>
      <td>Land</td>
      <td>184.00</td>
    </tr>
    <tr>
      <th>76</th>
      <td>hedron archive</td>
      <td>Artifact</td>
      <td>178.00</td>
    </tr>
    <tr>
      <th>79</th>
      <td>buried ruin</td>
      <td>Land</td>
      <td>174.00</td>
    </tr>
    <tr>
      <th>41</th>
      <td>oblivion ring</td>
      <td>Enchantment</td>
      <td>172.73</td>
    </tr>
    <tr>
      <th>7</th>
      <td>command tower</td>
      <td>Land</td>
      <td>170.71</td>
    </tr>
    <tr>
      <th>84</th>
      <td>vandalblast</td>
      <td>Sorcery</td>
      <td>170.00</td>
    </tr>
    <tr>
      <th>91</th>
      <td>reclamation sage</td>
      <td>Creature — Elf Shaman</td>
      <td>166.00</td>
    </tr>
    <tr>
      <th>90</th>
      <td>blue sun's zenith</td>
      <td>Instant</td>
      <td>166.00</td>
    </tr>
    <tr>
      <th>93</th>
      <td>dissipate</td>
      <td>Instant</td>
      <td>162.00</td>
    </tr>
    <tr>
      <th>47</th>
      <td>krosan grip</td>
      <td>Instant</td>
      <td>161.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
mean = numpy.mean(df['Value'])
std = numpy.std(df['Value'])
median = numpy.median(df['Value'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 9.697279824681504
    Standard Deviation: 24.832352551986812
    Median: 4.0


# Occurences by Color


```python
d = {'Title': edh['title'], 
     'Commanders': edh['occurences'], 
     'Color': edh['colors']}
df = pandas.DataFrame(data=d)

df = df.sort_values('Commanders', ascending=0)
```

## Colorless


```python
df[df['Color'] == 'C'][:50]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>C</td>
      <td>489</td>
      <td>sol ring</td>
    </tr>
    <tr>
      <th>1</th>
      <td>C</td>
      <td>394</td>
      <td>temple of the false god</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>373</td>
      <td>lightning greaves</td>
    </tr>
    <tr>
      <th>3</th>
      <td>C</td>
      <td>365</td>
      <td>swiftfoot boots</td>
    </tr>
    <tr>
      <th>4</th>
      <td>C</td>
      <td>349</td>
      <td>reliquary tower</td>
    </tr>
    <tr>
      <th>5</th>
      <td>C</td>
      <td>328</td>
      <td>solemn simulacrum</td>
    </tr>
    <tr>
      <th>6</th>
      <td>C</td>
      <td>252</td>
      <td>evolving wilds</td>
    </tr>
    <tr>
      <th>7</th>
      <td>C</td>
      <td>239</td>
      <td>command tower</td>
    </tr>
    <tr>
      <th>8</th>
      <td>C</td>
      <td>232</td>
      <td>terramorphic expanse</td>
    </tr>
    <tr>
      <th>9</th>
      <td>C</td>
      <td>232</td>
      <td>caged sun</td>
    </tr>
    <tr>
      <th>10</th>
      <td>C</td>
      <td>229</td>
      <td>gilded lotus</td>
    </tr>
    <tr>
      <th>11</th>
      <td>C</td>
      <td>220</td>
      <td>nykthos, shrine to nyx</td>
    </tr>
    <tr>
      <th>12</th>
      <td>C</td>
      <td>187</td>
      <td>burnished hart</td>
    </tr>
    <tr>
      <th>13</th>
      <td>C</td>
      <td>186</td>
      <td>thran dynamo</td>
    </tr>
    <tr>
      <th>14</th>
      <td>C</td>
      <td>184</td>
      <td>darksteel ingot</td>
    </tr>
    <tr>
      <th>15</th>
      <td>C</td>
      <td>181</td>
      <td>sensei's divining top</td>
    </tr>
    <tr>
      <th>16</th>
      <td>C</td>
      <td>177</td>
      <td>expedition map</td>
    </tr>
    <tr>
      <th>17</th>
      <td>C</td>
      <td>177</td>
      <td>myriad landscape</td>
    </tr>
    <tr>
      <th>18</th>
      <td>C</td>
      <td>176</td>
      <td>skullclamp</td>
    </tr>
    <tr>
      <th>22</th>
      <td>C</td>
      <td>159</td>
      <td>bojuka bog</td>
    </tr>
    <tr>
      <th>24</th>
      <td>C</td>
      <td>154</td>
      <td>commander's sphere</td>
    </tr>
    <tr>
      <th>25</th>
      <td>C</td>
      <td>154</td>
      <td>arcane lighthouse</td>
    </tr>
    <tr>
      <th>28</th>
      <td>C</td>
      <td>150</td>
      <td>urborg, tomb of yawgmoth</td>
    </tr>
    <tr>
      <th>29</th>
      <td>C</td>
      <td>149</td>
      <td>mind stone</td>
    </tr>
    <tr>
      <th>31</th>
      <td>C</td>
      <td>148</td>
      <td>strip mine</td>
    </tr>
    <tr>
      <th>33</th>
      <td>C</td>
      <td>148</td>
      <td>gauntlet of power</td>
    </tr>
    <tr>
      <th>37</th>
      <td>C</td>
      <td>142</td>
      <td>rogue's passage</td>
    </tr>
    <tr>
      <th>40</th>
      <td>C</td>
      <td>136</td>
      <td>extraplanar lens</td>
    </tr>
    <tr>
      <th>42</th>
      <td>C</td>
      <td>131</td>
      <td>thought vessel</td>
    </tr>
    <tr>
      <th>46</th>
      <td>C</td>
      <td>122</td>
      <td>ghost quarter</td>
    </tr>
    <tr>
      <th>51</th>
      <td>C</td>
      <td>115</td>
      <td>chromatic lantern</td>
    </tr>
    <tr>
      <th>59</th>
      <td>C</td>
      <td>101</td>
      <td>steel hellkite</td>
    </tr>
    <tr>
      <th>63</th>
      <td>C</td>
      <td>98</td>
      <td>everflowing chalice</td>
    </tr>
    <tr>
      <th>64</th>
      <td>C</td>
      <td>98</td>
      <td>worn powerstone</td>
    </tr>
    <tr>
      <th>66</th>
      <td>C</td>
      <td>98</td>
      <td>darksteel plate</td>
    </tr>
    <tr>
      <th>70</th>
      <td>C</td>
      <td>92</td>
      <td>halimar depths</td>
    </tr>
    <tr>
      <th>71</th>
      <td>C</td>
      <td>92</td>
      <td>nevinyrral's disk</td>
    </tr>
    <tr>
      <th>72</th>
      <td>C</td>
      <td>91</td>
      <td>cabal coffers</td>
    </tr>
    <tr>
      <th>76</th>
      <td>C</td>
      <td>89</td>
      <td>hedron archive</td>
    </tr>
    <tr>
      <th>79</th>
      <td>C</td>
      <td>87</td>
      <td>buried ruin</td>
    </tr>
    <tr>
      <th>83</th>
      <td>C</td>
      <td>86</td>
      <td>mimic vat</td>
    </tr>
    <tr>
      <th>88</th>
      <td>C</td>
      <td>84</td>
      <td>mind's eye</td>
    </tr>
    <tr>
      <th>101</th>
      <td>C</td>
      <td>79</td>
      <td>sword of feast and famine</td>
    </tr>
    <tr>
      <th>103</th>
      <td>C</td>
      <td>78</td>
      <td>elixir of immortality</td>
    </tr>
    <tr>
      <th>109</th>
      <td>C</td>
      <td>75</td>
      <td>whispersilk cloak</td>
    </tr>
    <tr>
      <th>110</th>
      <td>C</td>
      <td>75</td>
      <td>loxodon warhammer</td>
    </tr>
    <tr>
      <th>111</th>
      <td>C</td>
      <td>75</td>
      <td>sword of the animist</td>
    </tr>
    <tr>
      <th>114</th>
      <td>C</td>
      <td>74</td>
      <td>staff of nin</td>
    </tr>
    <tr>
      <th>120</th>
      <td>C</td>
      <td>72</td>
      <td>mana vault</td>
    </tr>
    <tr>
      <th>122</th>
      <td>C</td>
      <td>71</td>
      <td>thespian's stage</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'C']
mean = numpy.mean(dfstats['Commanders'])
std = numpy.std(dfstats['Commanders'])
median = numpy.median(dfstats['Commanders'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 18.0
    Standard Deviation: 38.54764293794291
    Median: 6.0


## White


```python
df[df['Color'] == 'W'][:25]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>23</th>
      <td>W</td>
      <td>158</td>
      <td>swords to plowshares</td>
    </tr>
    <tr>
      <th>32</th>
      <td>W</td>
      <td>148</td>
      <td>path to exile</td>
    </tr>
    <tr>
      <th>35</th>
      <td>W</td>
      <td>146</td>
      <td>sun titan</td>
    </tr>
    <tr>
      <th>39</th>
      <td>W</td>
      <td>136</td>
      <td>wrath of god</td>
    </tr>
    <tr>
      <th>41</th>
      <td>W</td>
      <td>133</td>
      <td>oblivion ring</td>
    </tr>
    <tr>
      <th>43</th>
      <td>W</td>
      <td>131</td>
      <td>return to dust</td>
    </tr>
    <tr>
      <th>48</th>
      <td>W</td>
      <td>121</td>
      <td>austere command</td>
    </tr>
    <tr>
      <th>50</th>
      <td>W</td>
      <td>117</td>
      <td>enlightened tutor</td>
    </tr>
    <tr>
      <th>52</th>
      <td>W</td>
      <td>110</td>
      <td>day of judgment</td>
    </tr>
    <tr>
      <th>99</th>
      <td>W</td>
      <td>79</td>
      <td>condemn</td>
    </tr>
    <tr>
      <th>113</th>
      <td>W</td>
      <td>75</td>
      <td>elspeth, sun's champion</td>
    </tr>
    <tr>
      <th>121</th>
      <td>W</td>
      <td>72</td>
      <td>land tax</td>
    </tr>
    <tr>
      <th>124</th>
      <td>W</td>
      <td>70</td>
      <td>grand abolisher</td>
    </tr>
    <tr>
      <th>128</th>
      <td>W</td>
      <td>70</td>
      <td>elesh norn, grand cenobite</td>
    </tr>
    <tr>
      <th>130</th>
      <td>W</td>
      <td>69</td>
      <td>true conviction</td>
    </tr>
    <tr>
      <th>142</th>
      <td>W</td>
      <td>64</td>
      <td>elspeth, knight-errant</td>
    </tr>
    <tr>
      <th>153</th>
      <td>W</td>
      <td>61</td>
      <td>avacyn, angel of hope</td>
    </tr>
    <tr>
      <th>156</th>
      <td>W</td>
      <td>60</td>
      <td>karmic guide</td>
    </tr>
    <tr>
      <th>162</th>
      <td>W</td>
      <td>59</td>
      <td>oblation</td>
    </tr>
    <tr>
      <th>163</th>
      <td>W</td>
      <td>59</td>
      <td>ghostly prison</td>
    </tr>
    <tr>
      <th>205</th>
      <td>W</td>
      <td>52</td>
      <td>mother of runes</td>
    </tr>
    <tr>
      <th>202</th>
      <td>W</td>
      <td>52</td>
      <td>emeria shepherd</td>
    </tr>
    <tr>
      <th>215</th>
      <td>W</td>
      <td>50</td>
      <td>cathars' crusade</td>
    </tr>
    <tr>
      <th>235</th>
      <td>W</td>
      <td>49</td>
      <td>weathered wayfarer</td>
    </tr>
    <tr>
      <th>232</th>
      <td>W</td>
      <td>49</td>
      <td>luminarch ascension</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'W']
mean = numpy.mean(dfstats['Commanders'])
std = numpy.std(dfstats['Commanders'])
median = numpy.median(dfstats['Commanders'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 7.431226765799257
    Standard Deviation: 15.675256671877563
    Median: 2.0


## Blue

* Hinder and Spell Crumple still see strong play despite the changes to the tuck rule.
* Preordain, Brainstorm, Ponder are not considered Commander staples, but find their way into the top 25 blue cards.


```python
df[df['Color'] == 'U'][:25]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>21</th>
      <td>U</td>
      <td>160</td>
      <td>cyclonic rift</td>
    </tr>
    <tr>
      <th>34</th>
      <td>U</td>
      <td>146</td>
      <td>counterspell</td>
    </tr>
    <tr>
      <th>36</th>
      <td>U</td>
      <td>143</td>
      <td>rhystic study</td>
    </tr>
    <tr>
      <th>58</th>
      <td>U</td>
      <td>105</td>
      <td>phyrexian metamorph</td>
    </tr>
    <tr>
      <th>56</th>
      <td>U</td>
      <td>105</td>
      <td>brainstorm</td>
    </tr>
    <tr>
      <th>61</th>
      <td>U</td>
      <td>100</td>
      <td>rite of replication</td>
    </tr>
    <tr>
      <th>62</th>
      <td>U</td>
      <td>99</td>
      <td>clever impersonator</td>
    </tr>
    <tr>
      <th>65</th>
      <td>U</td>
      <td>98</td>
      <td>fact or fiction</td>
    </tr>
    <tr>
      <th>69</th>
      <td>U</td>
      <td>96</td>
      <td>mystical tutor</td>
    </tr>
    <tr>
      <th>80</th>
      <td>U</td>
      <td>87</td>
      <td>ponder</td>
    </tr>
    <tr>
      <th>81</th>
      <td>U</td>
      <td>87</td>
      <td>hinder</td>
    </tr>
    <tr>
      <th>90</th>
      <td>U</td>
      <td>83</td>
      <td>blue sun's zenith</td>
    </tr>
    <tr>
      <th>92</th>
      <td>U</td>
      <td>82</td>
      <td>propaganda</td>
    </tr>
    <tr>
      <th>93</th>
      <td>U</td>
      <td>81</td>
      <td>dissipate</td>
    </tr>
    <tr>
      <th>102</th>
      <td>U</td>
      <td>79</td>
      <td>consecrated sphinx</td>
    </tr>
    <tr>
      <th>112</th>
      <td>U</td>
      <td>75</td>
      <td>spell crumple</td>
    </tr>
    <tr>
      <th>117</th>
      <td>U</td>
      <td>73</td>
      <td>capsize</td>
    </tr>
    <tr>
      <th>126</th>
      <td>U</td>
      <td>70</td>
      <td>thassa, god of the sea</td>
    </tr>
    <tr>
      <th>141</th>
      <td>U</td>
      <td>64</td>
      <td>fabricate</td>
    </tr>
    <tr>
      <th>145</th>
      <td>U</td>
      <td>63</td>
      <td>trinket mage</td>
    </tr>
    <tr>
      <th>144</th>
      <td>U</td>
      <td>63</td>
      <td>mulldrifter</td>
    </tr>
    <tr>
      <th>149</th>
      <td>U</td>
      <td>62</td>
      <td>preordain</td>
    </tr>
    <tr>
      <th>159</th>
      <td>U</td>
      <td>60</td>
      <td>leyline of anticipation</td>
    </tr>
    <tr>
      <th>154</th>
      <td>U</td>
      <td>60</td>
      <td>rewind</td>
    </tr>
    <tr>
      <th>166</th>
      <td>U</td>
      <td>59</td>
      <td>cryptic command</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'U']
mean = numpy.mean(dfstats['Commanders'])
std = numpy.std(dfstats['Commanders'])
median = numpy.median(dfstats['Commanders'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 6.897548161120841
    Standard Deviation: 15.038217896117773
    Median: 2.0


## Black

* Black is the only color that has a card in the top 20.
* Vampiric Tutor is functionally similar to Demonic Tutor and Diabolic Tutor.
* Demonic Tutor is the highest ranked tutor card.



```python
df[df['Color'] == 'B'][:25]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>19</th>
      <td>B</td>
      <td>170</td>
      <td>demonic tutor</td>
    </tr>
    <tr>
      <th>20</th>
      <td>B</td>
      <td>166</td>
      <td>diabolic tutor</td>
    </tr>
    <tr>
      <th>27</th>
      <td>B</td>
      <td>152</td>
      <td>phyrexian arena</td>
    </tr>
    <tr>
      <th>53</th>
      <td>B</td>
      <td>109</td>
      <td>liliana vess</td>
    </tr>
    <tr>
      <th>57</th>
      <td>B</td>
      <td>105</td>
      <td>decree of pain</td>
    </tr>
    <tr>
      <th>67</th>
      <td>B</td>
      <td>98</td>
      <td>sheoldred, whispering one</td>
    </tr>
    <tr>
      <th>73</th>
      <td>B</td>
      <td>91</td>
      <td>damnation</td>
    </tr>
    <tr>
      <th>75</th>
      <td>B</td>
      <td>90</td>
      <td>crypt ghast</td>
    </tr>
    <tr>
      <th>85</th>
      <td>B</td>
      <td>85</td>
      <td>grave titan</td>
    </tr>
    <tr>
      <th>86</th>
      <td>B</td>
      <td>84</td>
      <td>black sun's zenith</td>
    </tr>
    <tr>
      <th>87</th>
      <td>B</td>
      <td>84</td>
      <td>rune-scarred demon</td>
    </tr>
    <tr>
      <th>94</th>
      <td>B</td>
      <td>81</td>
      <td>hero's downfall</td>
    </tr>
    <tr>
      <th>95</th>
      <td>B</td>
      <td>81</td>
      <td>dictate of erebos</td>
    </tr>
    <tr>
      <th>97</th>
      <td>B</td>
      <td>81</td>
      <td>vampiric tutor</td>
    </tr>
    <tr>
      <th>98</th>
      <td>B</td>
      <td>80</td>
      <td>sidisi, undead vizier</td>
    </tr>
    <tr>
      <th>100</th>
      <td>B</td>
      <td>79</td>
      <td>whip of erebos</td>
    </tr>
    <tr>
      <th>107</th>
      <td>B</td>
      <td>76</td>
      <td>erebos, god of the dead</td>
    </tr>
    <tr>
      <th>108</th>
      <td>B</td>
      <td>76</td>
      <td>grave pact</td>
    </tr>
    <tr>
      <th>115</th>
      <td>B</td>
      <td>74</td>
      <td>exsanguinate</td>
    </tr>
    <tr>
      <th>116</th>
      <td>B</td>
      <td>73</td>
      <td>increasing ambition</td>
    </tr>
    <tr>
      <th>118</th>
      <td>B</td>
      <td>72</td>
      <td>bloodgift demon</td>
    </tr>
    <tr>
      <th>119</th>
      <td>B</td>
      <td>72</td>
      <td>go for the throat</td>
    </tr>
    <tr>
      <th>131</th>
      <td>B</td>
      <td>68</td>
      <td>gray merchant of asphodel</td>
    </tr>
    <tr>
      <th>138</th>
      <td>B</td>
      <td>66</td>
      <td>fleshbag marauder</td>
    </tr>
    <tr>
      <th>143</th>
      <td>B</td>
      <td>63</td>
      <td>read the bones</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'B']
mean = numpy.mean(dfstats['Commanders'])
std = numpy.std(dfstats['Commanders'])
median = numpy.median(dfstats['Commanders'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 9.044421487603305
    Standard Deviation: 17.851982373543997
    Median: 3.0


## Red

* Red doesn't have any colors in the top 50.
* There are no tutors in the top 25.


```python
df[df['Color'] == 'R'][:25]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>54</th>
      <td>R</td>
      <td>107</td>
      <td>chaos warp</td>
    </tr>
    <tr>
      <th>55</th>
      <td>R</td>
      <td>107</td>
      <td>blasphemous act</td>
    </tr>
    <tr>
      <th>84</th>
      <td>R</td>
      <td>85</td>
      <td>vandalblast</td>
    </tr>
    <tr>
      <th>146</th>
      <td>R</td>
      <td>63</td>
      <td>anger</td>
    </tr>
    <tr>
      <th>179</th>
      <td>R</td>
      <td>56</td>
      <td>reforge the soul</td>
    </tr>
    <tr>
      <th>191</th>
      <td>R</td>
      <td>54</td>
      <td>purphoros, god of the forge</td>
    </tr>
    <tr>
      <th>231</th>
      <td>R</td>
      <td>49</td>
      <td>insurrection</td>
    </tr>
    <tr>
      <th>237</th>
      <td>R</td>
      <td>48</td>
      <td>wild ricochet</td>
    </tr>
    <tr>
      <th>246</th>
      <td>R</td>
      <td>47</td>
      <td>warstorm surge</td>
    </tr>
    <tr>
      <th>266</th>
      <td>R</td>
      <td>45</td>
      <td>faithless looting</td>
    </tr>
    <tr>
      <th>304</th>
      <td>R</td>
      <td>43</td>
      <td>wheel of fortune</td>
    </tr>
    <tr>
      <th>295</th>
      <td>R</td>
      <td>43</td>
      <td>inferno titan</td>
    </tr>
    <tr>
      <th>294</th>
      <td>R</td>
      <td>43</td>
      <td>hammer of purphoros</td>
    </tr>
    <tr>
      <th>339</th>
      <td>R</td>
      <td>40</td>
      <td>urabrask the hidden</td>
    </tr>
    <tr>
      <th>342</th>
      <td>R</td>
      <td>39</td>
      <td>reverberate</td>
    </tr>
    <tr>
      <th>384</th>
      <td>R</td>
      <td>36</td>
      <td>ogre battledriver</td>
    </tr>
    <tr>
      <th>397</th>
      <td>R</td>
      <td>35</td>
      <td>comet storm</td>
    </tr>
    <tr>
      <th>429</th>
      <td>R</td>
      <td>34</td>
      <td>balefire dragon</td>
    </tr>
    <tr>
      <th>427</th>
      <td>R</td>
      <td>34</td>
      <td>hellkite tyrant</td>
    </tr>
    <tr>
      <th>450</th>
      <td>R</td>
      <td>33</td>
      <td>koth of the hammer</td>
    </tr>
    <tr>
      <th>455</th>
      <td>R</td>
      <td>33</td>
      <td>kiki-jiki, mirror breaker</td>
    </tr>
    <tr>
      <th>441</th>
      <td>R</td>
      <td>33</td>
      <td>dictate of the twin gods</td>
    </tr>
    <tr>
      <th>438</th>
      <td>R</td>
      <td>33</td>
      <td>outpost siege</td>
    </tr>
    <tr>
      <th>496</th>
      <td>R</td>
      <td>31</td>
      <td>gratuitous violence</td>
    </tr>
    <tr>
      <th>483</th>
      <td>R</td>
      <td>31</td>
      <td>zealous conscripts</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'R']
mean = numpy.mean(dfstats['Commanders'])
std = numpy.std(dfstats['Commanders'])
median = numpy.median(dfstats['Commanders'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 5.695278969957082
    Standard Deviation: 9.564635507429793
    Median: 2.0


## Green

* Green has eight tutors in the top 25, the most of any color.


```python
df[df['Color'] == 'G'][:25]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>26</th>
      <td>G</td>
      <td>153</td>
      <td>cultivate</td>
    </tr>
    <tr>
      <th>30</th>
      <td>G</td>
      <td>148</td>
      <td>kodama's reach</td>
    </tr>
    <tr>
      <th>38</th>
      <td>G</td>
      <td>140</td>
      <td>eternal witness</td>
    </tr>
    <tr>
      <th>44</th>
      <td>G</td>
      <td>127</td>
      <td>beast within</td>
    </tr>
    <tr>
      <th>45</th>
      <td>G</td>
      <td>126</td>
      <td>sakura-tribe elder</td>
    </tr>
    <tr>
      <th>47</th>
      <td>G</td>
      <td>121</td>
      <td>krosan grip</td>
    </tr>
    <tr>
      <th>49</th>
      <td>G</td>
      <td>119</td>
      <td>acidic slime</td>
    </tr>
    <tr>
      <th>60</th>
      <td>G</td>
      <td>101</td>
      <td>green sun's zenith</td>
    </tr>
    <tr>
      <th>68</th>
      <td>G</td>
      <td>98</td>
      <td>oracle of mul daya</td>
    </tr>
    <tr>
      <th>74</th>
      <td>G</td>
      <td>90</td>
      <td>explosive vegetation</td>
    </tr>
    <tr>
      <th>77</th>
      <td>G</td>
      <td>89</td>
      <td>harmonize</td>
    </tr>
    <tr>
      <th>78</th>
      <td>G</td>
      <td>88</td>
      <td>garruk wildspeaker</td>
    </tr>
    <tr>
      <th>82</th>
      <td>G</td>
      <td>87</td>
      <td>asceticism</td>
    </tr>
    <tr>
      <th>89</th>
      <td>G</td>
      <td>84</td>
      <td>sylvan library</td>
    </tr>
    <tr>
      <th>91</th>
      <td>G</td>
      <td>83</td>
      <td>reclamation sage</td>
    </tr>
    <tr>
      <th>96</th>
      <td>G</td>
      <td>81</td>
      <td>avenger of zendikar</td>
    </tr>
    <tr>
      <th>104</th>
      <td>G</td>
      <td>77</td>
      <td>skyshroud claim</td>
    </tr>
    <tr>
      <th>105</th>
      <td>G</td>
      <td>76</td>
      <td>yavimaya elder</td>
    </tr>
    <tr>
      <th>106</th>
      <td>G</td>
      <td>76</td>
      <td>wood elves</td>
    </tr>
    <tr>
      <th>123</th>
      <td>G</td>
      <td>70</td>
      <td>rampant growth</td>
    </tr>
    <tr>
      <th>127</th>
      <td>G</td>
      <td>70</td>
      <td>birds of paradise</td>
    </tr>
    <tr>
      <th>135</th>
      <td>G</td>
      <td>67</td>
      <td>chord of calling</td>
    </tr>
    <tr>
      <th>137</th>
      <td>G</td>
      <td>66</td>
      <td>terastodon</td>
    </tr>
    <tr>
      <th>139</th>
      <td>G</td>
      <td>65</td>
      <td>worldly tutor</td>
    </tr>
    <tr>
      <th>150</th>
      <td>G</td>
      <td>62</td>
      <td>tooth and nail</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'G']
mean = numpy.mean(dfstats['Commanders'])
std = numpy.std(dfstats['Commanders'])
median = numpy.median(dfstats['Commanders'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 8.590405904059041
    Standard Deviation: 16.843433033874295
    Median: 3.0


## WU (Azorius)


```python
df[df['Color'] == 'WU'][:10]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>254</th>
      <td>WU</td>
      <td>47</td>
      <td>supreme verdict</td>
    </tr>
    <tr>
      <th>378</th>
      <td>WU</td>
      <td>37</td>
      <td>venser, the sojourner</td>
    </tr>
    <tr>
      <th>436</th>
      <td>WU</td>
      <td>33</td>
      <td>render silent</td>
    </tr>
    <tr>
      <th>462</th>
      <td>WU</td>
      <td>32</td>
      <td>detention sphere</td>
    </tr>
    <tr>
      <th>518</th>
      <td>WU</td>
      <td>30</td>
      <td>sphinx's revelation</td>
    </tr>
    <tr>
      <th>807</th>
      <td>WU</td>
      <td>20</td>
      <td>narset transcendent</td>
    </tr>
    <tr>
      <th>1149</th>
      <td>WU</td>
      <td>14</td>
      <td>steel of the godhead</td>
    </tr>
    <tr>
      <th>1339</th>
      <td>WU</td>
      <td>12</td>
      <td>grand arbiter augustin iv</td>
    </tr>
    <tr>
      <th>1312</th>
      <td>WU</td>
      <td>12</td>
      <td>ojutai's command</td>
    </tr>
    <tr>
      <th>1393</th>
      <td>WU</td>
      <td>11</td>
      <td>azorius charm</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'WU']
mean = numpy.mean(dfstats['Commanders'])
std = numpy.std(dfstats['Commanders'])
median = numpy.median(dfstats['Commanders'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 6.175675675675675
    Standard Deviation: 8.98400697351795
    Median: 2.0


## UB (Dimir)


```python
df[df['Color'] == 'UB'][:10]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>769</th>
      <td>UB</td>
      <td>20</td>
      <td>notion thief</td>
    </tr>
    <tr>
      <th>867</th>
      <td>UB</td>
      <td>18</td>
      <td>evil twin</td>
    </tr>
    <tr>
      <th>953</th>
      <td>UB</td>
      <td>17</td>
      <td>baleful strix</td>
    </tr>
    <tr>
      <th>944</th>
      <td>UB</td>
      <td>17</td>
      <td>wrexial, the risen deep</td>
    </tr>
    <tr>
      <th>1090</th>
      <td>UB</td>
      <td>15</td>
      <td>memory plunder</td>
    </tr>
    <tr>
      <th>1167</th>
      <td>UB</td>
      <td>14</td>
      <td>havengul lich</td>
    </tr>
    <tr>
      <th>1255</th>
      <td>UB</td>
      <td>13</td>
      <td>ashiok, nightmare weaver</td>
    </tr>
    <tr>
      <th>1325</th>
      <td>UB</td>
      <td>12</td>
      <td>consuming aberration</td>
    </tr>
    <tr>
      <th>1434</th>
      <td>UB</td>
      <td>11</td>
      <td>lazav, dimir mastermind</td>
    </tr>
    <tr>
      <th>1417</th>
      <td>UB</td>
      <td>11</td>
      <td>dimir doppelganger</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'UB']
mean = numpy.mean(dfstats['Commanders'])
std = numpy.std(dfstats['Commanders'])
median = numpy.median(dfstats['Commanders'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 4.860215053763441
    Standard Deviation: 4.346773362326279
    Median: 3.0


## BR (Rakdos)


```python
df[df['Color'] == 'BR'][:10]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>287</th>
      <td>BR</td>
      <td>44</td>
      <td>terminate</td>
    </tr>
    <tr>
      <th>363</th>
      <td>BR</td>
      <td>38</td>
      <td>dreadbore</td>
    </tr>
    <tr>
      <th>663</th>
      <td>BR</td>
      <td>23</td>
      <td>rakdos charm</td>
    </tr>
    <tr>
      <th>1169</th>
      <td>BR</td>
      <td>14</td>
      <td>mogis, god of slaughter</td>
    </tr>
    <tr>
      <th>1127</th>
      <td>BR</td>
      <td>14</td>
      <td>deathbringer thoctar</td>
    </tr>
    <tr>
      <th>1320</th>
      <td>BR</td>
      <td>12</td>
      <td>rakdos, lord of riots</td>
    </tr>
    <tr>
      <th>1291</th>
      <td>BR</td>
      <td>12</td>
      <td>sire of insanity</td>
    </tr>
    <tr>
      <th>1588</th>
      <td>BR</td>
      <td>10</td>
      <td>kolaghan's command</td>
    </tr>
    <tr>
      <th>1571</th>
      <td>BR</td>
      <td>10</td>
      <td>master of cruelties</td>
    </tr>
    <tr>
      <th>1673</th>
      <td>BR</td>
      <td>9</td>
      <td>rakdos's return</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'BR']
mean = numpy.mean(dfstats['Commanders'])
std = numpy.std(dfstats['Commanders'])
median = numpy.median(dfstats['Commanders'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 4.935897435897436
    Standard Deviation: 7.077120491486564
    Median: 2.5


## RG (Gruul)


```python
df[df['Color'] == 'RG'][:10]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>498</th>
      <td>RG</td>
      <td>31</td>
      <td>xenagos, god of revels</td>
    </tr>
    <tr>
      <th>484</th>
      <td>RG</td>
      <td>31</td>
      <td>hull breach</td>
    </tr>
    <tr>
      <th>533</th>
      <td>RG</td>
      <td>29</td>
      <td>xenagos, the reveler</td>
    </tr>
    <tr>
      <th>614</th>
      <td>RG</td>
      <td>25</td>
      <td>fires of yavimaya</td>
    </tr>
    <tr>
      <th>675</th>
      <td>RG</td>
      <td>23</td>
      <td>decimate</td>
    </tr>
    <tr>
      <th>705</th>
      <td>RG</td>
      <td>22</td>
      <td>mina and denn, wildborn</td>
    </tr>
    <tr>
      <th>856</th>
      <td>RG</td>
      <td>19</td>
      <td>arlinn kord</td>
    </tr>
    <tr>
      <th>906</th>
      <td>RG</td>
      <td>18</td>
      <td>domri rade</td>
    </tr>
    <tr>
      <th>961</th>
      <td>RG</td>
      <td>17</td>
      <td>sarkhan vol</td>
    </tr>
    <tr>
      <th>1040</th>
      <td>RG</td>
      <td>15</td>
      <td>savage ventmaw</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'RG']
mean = numpy.mean(dfstats['Commanders'])
std = numpy.std(dfstats['Commanders'])
median = numpy.median(dfstats['Commanders'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 6.666666666666667
    Standard Deviation: 7.562700869692289
    Median: 3.0


## WB (Orzhov)


```python
df[df['Color'] == 'WB'][:10]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>385</th>
      <td>WB</td>
      <td>36</td>
      <td>utter end</td>
    </tr>
    <tr>
      <th>409</th>
      <td>WB</td>
      <td>35</td>
      <td>mortify</td>
    </tr>
    <tr>
      <th>407</th>
      <td>WB</td>
      <td>35</td>
      <td>merciless eviction</td>
    </tr>
    <tr>
      <th>424</th>
      <td>WB</td>
      <td>34</td>
      <td>anguished unmaking</td>
    </tr>
    <tr>
      <th>868</th>
      <td>WB</td>
      <td>18</td>
      <td>ashen rider</td>
    </tr>
    <tr>
      <th>964</th>
      <td>WB</td>
      <td>17</td>
      <td>sorin, grim nemesis</td>
    </tr>
    <tr>
      <th>1027</th>
      <td>WB</td>
      <td>16</td>
      <td>sorin, lord of innistrad</td>
    </tr>
    <tr>
      <th>1106</th>
      <td>WB</td>
      <td>15</td>
      <td>athreos, god of passage</td>
    </tr>
    <tr>
      <th>1162</th>
      <td>WB</td>
      <td>14</td>
      <td>angel of despair</td>
    </tr>
    <tr>
      <th>1174</th>
      <td>WB</td>
      <td>14</td>
      <td>debtors' knell</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'WB']
mean = numpy.mean(dfstats['Commanders'])
std = numpy.std(dfstats['Commanders'])
median = numpy.median(dfstats['Commanders'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 8.357142857142858
    Standard Deviation: 8.937138746802122
    Median: 5.5


## UR (Izzet)


```python
df[df['Color'] == 'UR'][:10]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>745</th>
      <td>UR</td>
      <td>21</td>
      <td>counterflux</td>
    </tr>
    <tr>
      <th>1086</th>
      <td>UR</td>
      <td>15</td>
      <td>niv-mizzet, the firemind</td>
    </tr>
    <tr>
      <th>1256</th>
      <td>UR</td>
      <td>13</td>
      <td>ral zarek</td>
    </tr>
    <tr>
      <th>1351</th>
      <td>UR</td>
      <td>12</td>
      <td>keranos, god of storms</td>
    </tr>
    <tr>
      <th>1304</th>
      <td>UR</td>
      <td>12</td>
      <td>izzet charm</td>
    </tr>
    <tr>
      <th>1411</th>
      <td>UR</td>
      <td>11</td>
      <td>hypersonic dragon</td>
    </tr>
    <tr>
      <th>1408</th>
      <td>UR</td>
      <td>11</td>
      <td>goblin electromancer</td>
    </tr>
    <tr>
      <th>1397</th>
      <td>UR</td>
      <td>11</td>
      <td>dack's duplicate</td>
    </tr>
    <tr>
      <th>1741</th>
      <td>UR</td>
      <td>9</td>
      <td>dack fayden</td>
    </tr>
    <tr>
      <th>1622</th>
      <td>UR</td>
      <td>9</td>
      <td>melek, izzet paragon</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'UR']
mean = numpy.mean(dfstats['Commanders'])
std = numpy.std(dfstats['Commanders'])
median = numpy.median(dfstats['Commanders'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 4.8076923076923075
    Standard Deviation: 4.565920842074981
    Median: 3.0


## BG (Golgari)


```python
df[df['Color'] == 'BG'][:10]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>344</th>
      <td>BG</td>
      <td>39</td>
      <td>putrefy</td>
    </tr>
    <tr>
      <th>719</th>
      <td>BG</td>
      <td>22</td>
      <td>vraska the unseen</td>
    </tr>
    <tr>
      <th>809</th>
      <td>BG</td>
      <td>20</td>
      <td>maelstrom pulse</td>
    </tr>
    <tr>
      <th>849</th>
      <td>BG</td>
      <td>19</td>
      <td>deathrite shaman</td>
    </tr>
    <tr>
      <th>887</th>
      <td>BG</td>
      <td>18</td>
      <td>jarad, golgari lich lord</td>
    </tr>
    <tr>
      <th>896</th>
      <td>BG</td>
      <td>18</td>
      <td>pernicious deed</td>
    </tr>
    <tr>
      <th>1007</th>
      <td>BG</td>
      <td>16</td>
      <td>deadbridge chant</td>
    </tr>
    <tr>
      <th>1091</th>
      <td>BG</td>
      <td>15</td>
      <td>the gitrog monster</td>
    </tr>
    <tr>
      <th>1063</th>
      <td>BG</td>
      <td>15</td>
      <td>golgari charm</td>
    </tr>
    <tr>
      <th>1059</th>
      <td>BG</td>
      <td>15</td>
      <td>deathreap ritual</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'BG']
mean = numpy.mean(dfstats['Commanders'])
std = numpy.std(dfstats['Commanders'])
median = numpy.median(dfstats['Commanders'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 7.081967213114754
    Standard Deviation: 7.263867646044558
    Median: 4.0


## RW (Boros)


```python
df[df['Color'] == 'WR'][:10]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>470</th>
      <td>WR</td>
      <td>32</td>
      <td>boros charm</td>
    </tr>
    <tr>
      <th>604</th>
      <td>WR</td>
      <td>26</td>
      <td>iroas, god of victory</td>
    </tr>
    <tr>
      <th>626</th>
      <td>WR</td>
      <td>25</td>
      <td>aurelia, the warleader</td>
    </tr>
    <tr>
      <th>671</th>
      <td>WR</td>
      <td>23</td>
      <td>assemble the legion</td>
    </tr>
    <tr>
      <th>721</th>
      <td>WR</td>
      <td>22</td>
      <td>gisela, blade of goldnight</td>
    </tr>
    <tr>
      <th>857</th>
      <td>WR</td>
      <td>19</td>
      <td>nahiri, the harbinger</td>
    </tr>
    <tr>
      <th>1173</th>
      <td>WR</td>
      <td>14</td>
      <td>ajani vengeant</td>
    </tr>
    <tr>
      <th>1252</th>
      <td>WR</td>
      <td>13</td>
      <td>balefire liege</td>
    </tr>
    <tr>
      <th>1278</th>
      <td>WR</td>
      <td>12</td>
      <td>deflecting palm</td>
    </tr>
    <tr>
      <th>1284</th>
      <td>WR</td>
      <td>12</td>
      <td>legion's initiative</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'WR']
mean = numpy.mean(dfstats['Commanders'])
std = numpy.std(dfstats['Commanders'])
median = numpy.median(dfstats['Commanders'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 5.569444444444445
    Standard Deviation: 6.763518128099889
    Median: 3.0


## GU (Simic)


```python
df[df['Color'] == 'UG'][:10]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>410</th>
      <td>UG</td>
      <td>35</td>
      <td>prophet of kruphix</td>
    </tr>
    <tr>
      <th>699</th>
      <td>UG</td>
      <td>22</td>
      <td>coiling oracle</td>
    </tr>
    <tr>
      <th>751</th>
      <td>UG</td>
      <td>21</td>
      <td>progenitor mimic</td>
    </tr>
    <tr>
      <th>755</th>
      <td>UG</td>
      <td>21</td>
      <td>kruphix, god of horizons</td>
    </tr>
    <tr>
      <th>791</th>
      <td>UG</td>
      <td>20</td>
      <td>kiora, the crashing wave</td>
    </tr>
    <tr>
      <th>776</th>
      <td>UG</td>
      <td>20</td>
      <td>altered ego</td>
    </tr>
    <tr>
      <th>834</th>
      <td>UG</td>
      <td>19</td>
      <td>mystic snake</td>
    </tr>
    <tr>
      <th>871</th>
      <td>UG</td>
      <td>18</td>
      <td>prime speaker zegana</td>
    </tr>
    <tr>
      <th>978</th>
      <td>UG</td>
      <td>16</td>
      <td>plasm capture</td>
    </tr>
    <tr>
      <th>1085</th>
      <td>UG</td>
      <td>15</td>
      <td>kiora, master of the depths</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'UG']
mean = numpy.mean(dfstats['Commanders'])
std = numpy.std(dfstats['Commanders'])
median = numpy.median(dfstats['Commanders'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 7.84
    Standard Deviation: 7.510952003574515
    Median: 5.5


## Other
Three or more colors.


```python
mask = (df['Color'].str.len() >= 3)
dfother = df.loc[mask]
dfother[:10]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>907</th>
      <td>UBR</td>
      <td>18</td>
      <td>nicol bolas, planeswalker</td>
    </tr>
    <tr>
      <th>1305</th>
      <td>UBR</td>
      <td>12</td>
      <td>cruel ultimatum</td>
    </tr>
    <tr>
      <th>1419</th>
      <td>UBR</td>
      <td>11</td>
      <td>crosis's charm</td>
    </tr>
    <tr>
      <th>1537</th>
      <td>WUG</td>
      <td>10</td>
      <td>bant charm</td>
    </tr>
    <tr>
      <th>1565</th>
      <td>WUG</td>
      <td>10</td>
      <td>wargate</td>
    </tr>
    <tr>
      <th>1638</th>
      <td>WUB</td>
      <td>9</td>
      <td>magister sphinx</td>
    </tr>
    <tr>
      <th>1915</th>
      <td>WUBRG</td>
      <td>8</td>
      <td>maelstrom nexus</td>
    </tr>
    <tr>
      <th>1783</th>
      <td>WRG</td>
      <td>8</td>
      <td>naya charm</td>
    </tr>
    <tr>
      <th>1784</th>
      <td>WRG</td>
      <td>8</td>
      <td>titanic ultimatum</td>
    </tr>
    <tr>
      <th>2062</th>
      <td>WUB</td>
      <td>7</td>
      <td>sphinx of the steel wind</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = dfother
mean = numpy.mean(dfstats['Commanders'])
std = numpy.std(dfstats['Commanders'])
median = numpy.median(dfstats['Commanders'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 2.7245508982035926
    Standard Deviation: 2.460866578924113
    Median: 2.0


# Value by Color


```python
d = {'Title': edh['title'],
     'Price': edh['priceAdjusted'],
     'Commanders': edh['occurences'],
     'Value': edh['value'], 
     'Color': edh['colors']}
df = pandas.DataFrame(data=d)

df = df.sort_values('Value', ascending=0)
```

## Colorless


```python
df[df['Color'] == 'C'][:50]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Price</th>
      <th>Title</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>C</td>
      <td>394</td>
      <td>0.50</td>
      <td>temple of the false god</td>
      <td>788.00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>C</td>
      <td>365</td>
      <td>0.50</td>
      <td>swiftfoot boots</td>
      <td>730.00</td>
    </tr>
    <tr>
      <th>6</th>
      <td>C</td>
      <td>252</td>
      <td>0.50</td>
      <td>evolving wilds</td>
      <td>504.00</td>
    </tr>
    <tr>
      <th>8</th>
      <td>C</td>
      <td>232</td>
      <td>0.50</td>
      <td>terramorphic expanse</td>
      <td>464.00</td>
    </tr>
    <tr>
      <th>12</th>
      <td>C</td>
      <td>187</td>
      <td>0.50</td>
      <td>burnished hart</td>
      <td>374.00</td>
    </tr>
    <tr>
      <th>14</th>
      <td>C</td>
      <td>184</td>
      <td>0.50</td>
      <td>darksteel ingot</td>
      <td>368.00</td>
    </tr>
    <tr>
      <th>24</th>
      <td>C</td>
      <td>154</td>
      <td>0.50</td>
      <td>commander's sphere</td>
      <td>308.00</td>
    </tr>
    <tr>
      <th>29</th>
      <td>C</td>
      <td>149</td>
      <td>0.50</td>
      <td>mind stone</td>
      <td>298.00</td>
    </tr>
    <tr>
      <th>37</th>
      <td>C</td>
      <td>142</td>
      <td>0.50</td>
      <td>rogue's passage</td>
      <td>284.00</td>
    </tr>
    <tr>
      <th>59</th>
      <td>C</td>
      <td>101</td>
      <td>0.50</td>
      <td>steel hellkite</td>
      <td>202.00</td>
    </tr>
    <tr>
      <th>0</th>
      <td>C</td>
      <td>489</td>
      <td>2.49</td>
      <td>sol ring</td>
      <td>196.39</td>
    </tr>
    <tr>
      <th>63</th>
      <td>C</td>
      <td>98</td>
      <td>0.50</td>
      <td>everflowing chalice</td>
      <td>196.00</td>
    </tr>
    <tr>
      <th>22</th>
      <td>C</td>
      <td>159</td>
      <td>0.86</td>
      <td>bojuka bog</td>
      <td>184.88</td>
    </tr>
    <tr>
      <th>70</th>
      <td>C</td>
      <td>92</td>
      <td>0.50</td>
      <td>halimar depths</td>
      <td>184.00</td>
    </tr>
    <tr>
      <th>76</th>
      <td>C</td>
      <td>89</td>
      <td>0.50</td>
      <td>hedron archive</td>
      <td>178.00</td>
    </tr>
    <tr>
      <th>79</th>
      <td>C</td>
      <td>87</td>
      <td>0.50</td>
      <td>buried ruin</td>
      <td>174.00</td>
    </tr>
    <tr>
      <th>7</th>
      <td>C</td>
      <td>239</td>
      <td>1.40</td>
      <td>command tower</td>
      <td>170.71</td>
    </tr>
    <tr>
      <th>103</th>
      <td>C</td>
      <td>78</td>
      <td>0.50</td>
      <td>elixir of immortality</td>
      <td>156.00</td>
    </tr>
    <tr>
      <th>114</th>
      <td>C</td>
      <td>74</td>
      <td>0.50</td>
      <td>staff of nin</td>
      <td>148.00</td>
    </tr>
    <tr>
      <th>129</th>
      <td>C</td>
      <td>69</td>
      <td>0.50</td>
      <td>barren moor</td>
      <td>138.00</td>
    </tr>
    <tr>
      <th>133</th>
      <td>C</td>
      <td>67</td>
      <td>0.50</td>
      <td>secluded steppe</td>
      <td>134.00</td>
    </tr>
    <tr>
      <th>42</th>
      <td>C</td>
      <td>131</td>
      <td>0.98</td>
      <td>thought vessel</td>
      <td>133.67</td>
    </tr>
    <tr>
      <th>4</th>
      <td>C</td>
      <td>349</td>
      <td>2.84</td>
      <td>reliquary tower</td>
      <td>122.89</td>
    </tr>
    <tr>
      <th>151</th>
      <td>C</td>
      <td>61</td>
      <td>0.50</td>
      <td>oran-rief, the vastwood</td>
      <td>122.00</td>
    </tr>
    <tr>
      <th>172</th>
      <td>C</td>
      <td>57</td>
      <td>0.50</td>
      <td>forgotten cave</td>
      <td>114.00</td>
    </tr>
    <tr>
      <th>173</th>
      <td>C</td>
      <td>57</td>
      <td>0.50</td>
      <td>wayfarer's bauble</td>
      <td>114.00</td>
    </tr>
    <tr>
      <th>64</th>
      <td>C</td>
      <td>98</td>
      <td>0.89</td>
      <td>worn powerstone</td>
      <td>110.11</td>
    </tr>
    <tr>
      <th>183</th>
      <td>C</td>
      <td>55</td>
      <td>0.50</td>
      <td>trading post</td>
      <td>110.00</td>
    </tr>
    <tr>
      <th>188</th>
      <td>C</td>
      <td>54</td>
      <td>0.50</td>
      <td>lonely sandbar</td>
      <td>108.00</td>
    </tr>
    <tr>
      <th>25</th>
      <td>C</td>
      <td>154</td>
      <td>1.45</td>
      <td>arcane lighthouse</td>
      <td>106.21</td>
    </tr>
    <tr>
      <th>203</th>
      <td>C</td>
      <td>52</td>
      <td>0.50</td>
      <td>tranquil thicket</td>
      <td>104.00</td>
    </tr>
    <tr>
      <th>209</th>
      <td>C</td>
      <td>51</td>
      <td>0.50</td>
      <td>unstable obelisk</td>
      <td>102.00</td>
    </tr>
    <tr>
      <th>109</th>
      <td>C</td>
      <td>75</td>
      <td>0.75</td>
      <td>whispersilk cloak</td>
      <td>100.00</td>
    </tr>
    <tr>
      <th>18</th>
      <td>C</td>
      <td>176</td>
      <td>1.77</td>
      <td>skullclamp</td>
      <td>99.44</td>
    </tr>
    <tr>
      <th>236</th>
      <td>C</td>
      <td>48</td>
      <td>0.50</td>
      <td>crypt of agadeem</td>
      <td>96.00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>373</td>
      <td>3.94</td>
      <td>lightning greaves</td>
      <td>94.67</td>
    </tr>
    <tr>
      <th>247</th>
      <td>C</td>
      <td>47</td>
      <td>0.50</td>
      <td>blighted woodland</td>
      <td>94.00</td>
    </tr>
    <tr>
      <th>110</th>
      <td>C</td>
      <td>75</td>
      <td>0.83</td>
      <td>loxodon warhammer</td>
      <td>90.36</td>
    </tr>
    <tr>
      <th>16</th>
      <td>C</td>
      <td>177</td>
      <td>1.98</td>
      <td>expedition map</td>
      <td>89.39</td>
    </tr>
    <tr>
      <th>280</th>
      <td>C</td>
      <td>44</td>
      <td>0.50</td>
      <td>spine of ish sah</td>
      <td>88.00</td>
    </tr>
    <tr>
      <th>282</th>
      <td>C</td>
      <td>44</td>
      <td>0.50</td>
      <td>rakdos signet</td>
      <td>88.00</td>
    </tr>
    <tr>
      <th>293</th>
      <td>C</td>
      <td>43</td>
      <td>0.50</td>
      <td>mortuary mire</td>
      <td>86.00</td>
    </tr>
    <tr>
      <th>307</th>
      <td>C</td>
      <td>42</td>
      <td>0.50</td>
      <td>azorius chancery</td>
      <td>84.00</td>
    </tr>
    <tr>
      <th>46</th>
      <td>C</td>
      <td>122</td>
      <td>1.47</td>
      <td>ghost quarter</td>
      <td>82.99</td>
    </tr>
    <tr>
      <th>317</th>
      <td>C</td>
      <td>41</td>
      <td>0.50</td>
      <td>opal palace</td>
      <td>82.00</td>
    </tr>
    <tr>
      <th>319</th>
      <td>C</td>
      <td>41</td>
      <td>0.50</td>
      <td>kher keep</td>
      <td>82.00</td>
    </tr>
    <tr>
      <th>318</th>
      <td>C</td>
      <td>41</td>
      <td>0.50</td>
      <td>rakdos carnarium</td>
      <td>82.00</td>
    </tr>
    <tr>
      <th>320</th>
      <td>C</td>
      <td>41</td>
      <td>0.50</td>
      <td>selesnya sanctuary</td>
      <td>82.00</td>
    </tr>
    <tr>
      <th>5</th>
      <td>C</td>
      <td>328</td>
      <td>4.00</td>
      <td>solemn simulacrum</td>
      <td>82.00</td>
    </tr>
    <tr>
      <th>343</th>
      <td>C</td>
      <td>39</td>
      <td>0.50</td>
      <td>fireshrieker</td>
      <td>78.00</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'C']
mean = numpy.mean(dfstats['Value'])
std = numpy.std(dfstats['Value'])
median = numpy.median(dfstats['Value'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 17.270638641875514
    Standard Deviation: 47.778902031984686
    Median: 4.0


## White


```python
df[df['Color'] == 'W'][:25]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Price</th>
      <th>Title</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>41</th>
      <td>W</td>
      <td>133</td>
      <td>0.77</td>
      <td>oblivion ring</td>
      <td>172.73</td>
    </tr>
    <tr>
      <th>99</th>
      <td>W</td>
      <td>79</td>
      <td>0.50</td>
      <td>condemn</td>
      <td>158.00</td>
    </tr>
    <tr>
      <th>130</th>
      <td>W</td>
      <td>69</td>
      <td>0.50</td>
      <td>true conviction</td>
      <td>138.00</td>
    </tr>
    <tr>
      <th>162</th>
      <td>W</td>
      <td>59</td>
      <td>0.50</td>
      <td>oblation</td>
      <td>118.00</td>
    </tr>
    <tr>
      <th>43</th>
      <td>W</td>
      <td>131</td>
      <td>1.19</td>
      <td>return to dust</td>
      <td>110.08</td>
    </tr>
    <tr>
      <th>202</th>
      <td>W</td>
      <td>52</td>
      <td>0.50</td>
      <td>emeria shepherd</td>
      <td>104.00</td>
    </tr>
    <tr>
      <th>215</th>
      <td>W</td>
      <td>50</td>
      <td>0.50</td>
      <td>cathars' crusade</td>
      <td>100.00</td>
    </tr>
    <tr>
      <th>224</th>
      <td>W</td>
      <td>49</td>
      <td>0.50</td>
      <td>banishing light</td>
      <td>98.00</td>
    </tr>
    <tr>
      <th>249</th>
      <td>W</td>
      <td>47</td>
      <td>0.50</td>
      <td>marshal's anthem</td>
      <td>94.00</td>
    </tr>
    <tr>
      <th>268</th>
      <td>W</td>
      <td>45</td>
      <td>0.50</td>
      <td>mentor of the meek</td>
      <td>90.00</td>
    </tr>
    <tr>
      <th>281</th>
      <td>W</td>
      <td>44</td>
      <td>0.50</td>
      <td>spear of heliod</td>
      <td>88.00</td>
    </tr>
    <tr>
      <th>356</th>
      <td>W</td>
      <td>38</td>
      <td>0.50</td>
      <td>mass calcify</td>
      <td>76.00</td>
    </tr>
    <tr>
      <th>382</th>
      <td>W</td>
      <td>36</td>
      <td>0.50</td>
      <td>angel of serenity</td>
      <td>72.00</td>
    </tr>
    <tr>
      <th>396</th>
      <td>W</td>
      <td>35</td>
      <td>0.50</td>
      <td>sunblast angel</td>
      <td>70.00</td>
    </tr>
    <tr>
      <th>406</th>
      <td>W</td>
      <td>35</td>
      <td>0.50</td>
      <td>sphere of safety</td>
      <td>70.00</td>
    </tr>
    <tr>
      <th>35</th>
      <td>W</td>
      <td>146</td>
      <td>2.10</td>
      <td>sun titan</td>
      <td>69.52</td>
    </tr>
    <tr>
      <th>444</th>
      <td>W</td>
      <td>33</td>
      <td>0.50</td>
      <td>open the armory</td>
      <td>66.00</td>
    </tr>
    <tr>
      <th>442</th>
      <td>W</td>
      <td>33</td>
      <td>0.50</td>
      <td>fiend hunter</td>
      <td>66.00</td>
    </tr>
    <tr>
      <th>23</th>
      <td>W</td>
      <td>158</td>
      <td>2.42</td>
      <td>swords to plowshares</td>
      <td>65.29</td>
    </tr>
    <tr>
      <th>537</th>
      <td>W</td>
      <td>28</td>
      <td>0.50</td>
      <td>silverblade paladin</td>
      <td>56.00</td>
    </tr>
    <tr>
      <th>552</th>
      <td>W</td>
      <td>27</td>
      <td>0.50</td>
      <td>angelic skirmisher</td>
      <td>54.00</td>
    </tr>
    <tr>
      <th>583</th>
      <td>W</td>
      <td>26</td>
      <td>0.50</td>
      <td>gift of immortality</td>
      <td>52.00</td>
    </tr>
    <tr>
      <th>588</th>
      <td>W</td>
      <td>26</td>
      <td>0.50</td>
      <td>adarkar valkyrie</td>
      <td>52.00</td>
    </tr>
    <tr>
      <th>52</th>
      <td>W</td>
      <td>110</td>
      <td>2.20</td>
      <td>day of judgment</td>
      <td>50.00</td>
    </tr>
    <tr>
      <th>609</th>
      <td>W</td>
      <td>25</td>
      <td>0.50</td>
      <td>captain of the watch</td>
      <td>50.00</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'W']
mean = numpy.mean(dfstats['Value'])
std = numpy.std(dfstats['Value'])
median = numpy.median(dfstats['Value'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 7.940585501858733
    Standard Deviation: 15.15487508920651
    Median: 2.985


## Blue


```python
df[df['Color'] == 'U'][:25]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Price</th>
      <th>Title</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>90</th>
      <td>U</td>
      <td>83</td>
      <td>0.50</td>
      <td>blue sun's zenith</td>
      <td>166.00</td>
    </tr>
    <tr>
      <th>93</th>
      <td>U</td>
      <td>81</td>
      <td>0.50</td>
      <td>dissipate</td>
      <td>162.00</td>
    </tr>
    <tr>
      <th>34</th>
      <td>U</td>
      <td>146</td>
      <td>1.00</td>
      <td>counterspell</td>
      <td>146.00</td>
    </tr>
    <tr>
      <th>144</th>
      <td>U</td>
      <td>63</td>
      <td>0.50</td>
      <td>mulldrifter</td>
      <td>126.00</td>
    </tr>
    <tr>
      <th>145</th>
      <td>U</td>
      <td>63</td>
      <td>0.50</td>
      <td>trinket mage</td>
      <td>126.00</td>
    </tr>
    <tr>
      <th>154</th>
      <td>U</td>
      <td>60</td>
      <td>0.50</td>
      <td>rewind</td>
      <td>120.00</td>
    </tr>
    <tr>
      <th>176</th>
      <td>U</td>
      <td>56</td>
      <td>0.50</td>
      <td>archaeomancer</td>
      <td>112.00</td>
    </tr>
    <tr>
      <th>56</th>
      <td>U</td>
      <td>105</td>
      <td>0.95</td>
      <td>brainstorm</td>
      <td>110.53</td>
    </tr>
    <tr>
      <th>189</th>
      <td>U</td>
      <td>54</td>
      <td>0.50</td>
      <td>dissolve</td>
      <td>108.00</td>
    </tr>
    <tr>
      <th>194</th>
      <td>U</td>
      <td>53</td>
      <td>0.50</td>
      <td>curse of the swine</td>
      <td>106.00</td>
    </tr>
    <tr>
      <th>210</th>
      <td>U</td>
      <td>51</td>
      <td>0.50</td>
      <td>treasure cruise</td>
      <td>102.00</td>
    </tr>
    <tr>
      <th>223</th>
      <td>U</td>
      <td>49</td>
      <td>0.50</td>
      <td>negate</td>
      <td>98.00</td>
    </tr>
    <tr>
      <th>258</th>
      <td>U</td>
      <td>46</td>
      <td>0.50</td>
      <td>dig through time</td>
      <td>92.00</td>
    </tr>
    <tr>
      <th>65</th>
      <td>U</td>
      <td>98</td>
      <td>1.09</td>
      <td>fact or fiction</td>
      <td>89.91</td>
    </tr>
    <tr>
      <th>310</th>
      <td>U</td>
      <td>42</td>
      <td>0.50</td>
      <td>rapid hybridization</td>
      <td>84.00</td>
    </tr>
    <tr>
      <th>341</th>
      <td>U</td>
      <td>39</td>
      <td>0.50</td>
      <td>stormtide leviathan</td>
      <td>78.00</td>
    </tr>
    <tr>
      <th>370</th>
      <td>U</td>
      <td>37</td>
      <td>0.50</td>
      <td>reality shift</td>
      <td>74.00</td>
    </tr>
    <tr>
      <th>371</th>
      <td>U</td>
      <td>37</td>
      <td>0.50</td>
      <td>swan song</td>
      <td>74.00</td>
    </tr>
    <tr>
      <th>408</th>
      <td>U</td>
      <td>35</td>
      <td>0.50</td>
      <td>high tide</td>
      <td>70.00</td>
    </tr>
    <tr>
      <th>416</th>
      <td>U</td>
      <td>34</td>
      <td>0.50</td>
      <td>bident of thassa</td>
      <td>68.00</td>
    </tr>
    <tr>
      <th>61</th>
      <td>U</td>
      <td>100</td>
      <td>1.52</td>
      <td>rite of replication</td>
      <td>65.79</td>
    </tr>
    <tr>
      <th>80</th>
      <td>U</td>
      <td>87</td>
      <td>1.33</td>
      <td>ponder</td>
      <td>65.41</td>
    </tr>
    <tr>
      <th>508</th>
      <td>U</td>
      <td>30</td>
      <td>0.50</td>
      <td>future sight</td>
      <td>60.00</td>
    </tr>
    <tr>
      <th>504</th>
      <td>U</td>
      <td>30</td>
      <td>0.50</td>
      <td>mana leak</td>
      <td>60.00</td>
    </tr>
    <tr>
      <th>524</th>
      <td>U</td>
      <td>29</td>
      <td>0.50</td>
      <td>cancel</td>
      <td>58.00</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'U']
mean = numpy.mean(dfstats['Value'])
std = numpy.std(dfstats['Value'])
median = numpy.median(dfstats['Value'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 7.9557618213660355
    Standard Deviation: 16.224316825945767
    Median: 2.275


## Black


```python
df[df['Color'] == 'B'][:25]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Price</th>
      <th>Title</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>20</th>
      <td>B</td>
      <td>166</td>
      <td>0.5</td>
      <td>diabolic tutor</td>
      <td>332.0</td>
    </tr>
    <tr>
      <th>116</th>
      <td>B</td>
      <td>73</td>
      <td>0.5</td>
      <td>increasing ambition</td>
      <td>146.0</td>
    </tr>
    <tr>
      <th>118</th>
      <td>B</td>
      <td>72</td>
      <td>0.5</td>
      <td>bloodgift demon</td>
      <td>144.0</td>
    </tr>
    <tr>
      <th>131</th>
      <td>B</td>
      <td>68</td>
      <td>0.5</td>
      <td>gray merchant of asphodel</td>
      <td>136.0</td>
    </tr>
    <tr>
      <th>138</th>
      <td>B</td>
      <td>66</td>
      <td>0.5</td>
      <td>fleshbag marauder</td>
      <td>132.0</td>
    </tr>
    <tr>
      <th>143</th>
      <td>B</td>
      <td>63</td>
      <td>0.5</td>
      <td>read the bones</td>
      <td>126.0</td>
    </tr>
    <tr>
      <th>148</th>
      <td>B</td>
      <td>62</td>
      <td>0.5</td>
      <td>butcher of malakir</td>
      <td>124.0</td>
    </tr>
    <tr>
      <th>167</th>
      <td>B</td>
      <td>58</td>
      <td>0.5</td>
      <td>sign in blood</td>
      <td>116.0</td>
    </tr>
    <tr>
      <th>168</th>
      <td>B</td>
      <td>58</td>
      <td>0.5</td>
      <td>doom blade</td>
      <td>116.0</td>
    </tr>
    <tr>
      <th>193</th>
      <td>B</td>
      <td>53</td>
      <td>0.5</td>
      <td>disciple of bolas</td>
      <td>106.0</td>
    </tr>
    <tr>
      <th>212</th>
      <td>B</td>
      <td>50</td>
      <td>0.5</td>
      <td>mutilate</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>214</th>
      <td>B</td>
      <td>50</td>
      <td>0.5</td>
      <td>murder</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>227</th>
      <td>B</td>
      <td>49</td>
      <td>0.5</td>
      <td>phyrexian reclamation</td>
      <td>98.0</td>
    </tr>
    <tr>
      <th>226</th>
      <td>B</td>
      <td>49</td>
      <td>0.5</td>
      <td>harvester of souls</td>
      <td>98.0</td>
    </tr>
    <tr>
      <th>238</th>
      <td>B</td>
      <td>48</td>
      <td>0.5</td>
      <td>archfiend of depravity</td>
      <td>96.0</td>
    </tr>
    <tr>
      <th>248</th>
      <td>B</td>
      <td>47</td>
      <td>0.5</td>
      <td>sepulchral primordial</td>
      <td>94.0</td>
    </tr>
    <tr>
      <th>257</th>
      <td>B</td>
      <td>46</td>
      <td>0.5</td>
      <td>grave betrayal</td>
      <td>92.0</td>
    </tr>
    <tr>
      <th>267</th>
      <td>B</td>
      <td>45</td>
      <td>0.5</td>
      <td>profane command</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>291</th>
      <td>B</td>
      <td>43</td>
      <td>0.5</td>
      <td>sudden spoiling</td>
      <td>86.0</td>
    </tr>
    <tr>
      <th>292</th>
      <td>B</td>
      <td>43</td>
      <td>0.5</td>
      <td>magus of the coffers</td>
      <td>86.0</td>
    </tr>
    <tr>
      <th>306</th>
      <td>B</td>
      <td>42</td>
      <td>0.5</td>
      <td>reassembling skeleton</td>
      <td>84.0</td>
    </tr>
    <tr>
      <th>308</th>
      <td>B</td>
      <td>42</td>
      <td>0.5</td>
      <td>victimize</td>
      <td>84.0</td>
    </tr>
    <tr>
      <th>321</th>
      <td>B</td>
      <td>41</td>
      <td>0.5</td>
      <td>tragic slip</td>
      <td>82.0</td>
    </tr>
    <tr>
      <th>316</th>
      <td>B</td>
      <td>41</td>
      <td>0.5</td>
      <td>palace siege</td>
      <td>82.0</td>
    </tr>
    <tr>
      <th>315</th>
      <td>B</td>
      <td>41</td>
      <td>0.5</td>
      <td>merciless executioner</td>
      <td>82.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'B']
mean = numpy.mean(dfstats['Value'])
std = numpy.std(dfstats['Value'])
median = numpy.median(dfstats['Value'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 9.864566115702477
    Standard Deviation: 21.352338028294376
    Median: 4.0


## Red


```python
df[df['Color'] == 'R'][:25]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Price</th>
      <th>Title</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>84</th>
      <td>R</td>
      <td>85</td>
      <td>0.5</td>
      <td>vandalblast</td>
      <td>170.0</td>
    </tr>
    <tr>
      <th>237</th>
      <td>R</td>
      <td>48</td>
      <td>0.5</td>
      <td>wild ricochet</td>
      <td>96.0</td>
    </tr>
    <tr>
      <th>246</th>
      <td>R</td>
      <td>47</td>
      <td>0.5</td>
      <td>warstorm surge</td>
      <td>94.0</td>
    </tr>
    <tr>
      <th>266</th>
      <td>R</td>
      <td>45</td>
      <td>0.5</td>
      <td>faithless looting</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>294</th>
      <td>R</td>
      <td>43</td>
      <td>0.5</td>
      <td>hammer of purphoros</td>
      <td>86.0</td>
    </tr>
    <tr>
      <th>295</th>
      <td>R</td>
      <td>43</td>
      <td>0.5</td>
      <td>inferno titan</td>
      <td>86.0</td>
    </tr>
    <tr>
      <th>342</th>
      <td>R</td>
      <td>39</td>
      <td>0.5</td>
      <td>reverberate</td>
      <td>78.0</td>
    </tr>
    <tr>
      <th>384</th>
      <td>R</td>
      <td>36</td>
      <td>0.5</td>
      <td>ogre battledriver</td>
      <td>72.0</td>
    </tr>
    <tr>
      <th>397</th>
      <td>R</td>
      <td>35</td>
      <td>0.5</td>
      <td>comet storm</td>
      <td>70.0</td>
    </tr>
    <tr>
      <th>441</th>
      <td>R</td>
      <td>33</td>
      <td>0.5</td>
      <td>dictate of the twin gods</td>
      <td>66.0</td>
    </tr>
    <tr>
      <th>438</th>
      <td>R</td>
      <td>33</td>
      <td>0.5</td>
      <td>outpost siege</td>
      <td>66.0</td>
    </tr>
    <tr>
      <th>480</th>
      <td>R</td>
      <td>31</td>
      <td>0.5</td>
      <td>feldon of the third path</td>
      <td>62.0</td>
    </tr>
    <tr>
      <th>483</th>
      <td>R</td>
      <td>31</td>
      <td>0.5</td>
      <td>zealous conscripts</td>
      <td>62.0</td>
    </tr>
    <tr>
      <th>510</th>
      <td>R</td>
      <td>30</td>
      <td>0.5</td>
      <td>hoarding dragon</td>
      <td>60.0</td>
    </tr>
    <tr>
      <th>540</th>
      <td>R</td>
      <td>28</td>
      <td>0.5</td>
      <td>mizzium mortars</td>
      <td>56.0</td>
    </tr>
    <tr>
      <th>555</th>
      <td>R</td>
      <td>27</td>
      <td>0.5</td>
      <td>hellkite charger</td>
      <td>54.0</td>
    </tr>
    <tr>
      <th>557</th>
      <td>R</td>
      <td>27</td>
      <td>0.5</td>
      <td>dualcaster mage</td>
      <td>54.0</td>
    </tr>
    <tr>
      <th>559</th>
      <td>R</td>
      <td>27</td>
      <td>0.5</td>
      <td>furnace of rath</td>
      <td>54.0</td>
    </tr>
    <tr>
      <th>584</th>
      <td>R</td>
      <td>26</td>
      <td>0.5</td>
      <td>hoard-smelter dragon</td>
      <td>52.0</td>
    </tr>
    <tr>
      <th>617</th>
      <td>R</td>
      <td>25</td>
      <td>0.5</td>
      <td>charmbreaker devils</td>
      <td>50.0</td>
    </tr>
    <tr>
      <th>611</th>
      <td>R</td>
      <td>25</td>
      <td>0.5</td>
      <td>vicious shadows</td>
      <td>50.0</td>
    </tr>
    <tr>
      <th>646</th>
      <td>R</td>
      <td>24</td>
      <td>0.5</td>
      <td>kazuul, tyrant of the cliffs</td>
      <td>48.0</td>
    </tr>
    <tr>
      <th>665</th>
      <td>R</td>
      <td>23</td>
      <td>0.5</td>
      <td>impact tremors</td>
      <td>46.0</td>
    </tr>
    <tr>
      <th>707</th>
      <td>R</td>
      <td>22</td>
      <td>0.5</td>
      <td>ruination</td>
      <td>44.0</td>
    </tr>
    <tr>
      <th>704</th>
      <td>R</td>
      <td>22</td>
      <td>0.5</td>
      <td>chandra's ignition</td>
      <td>44.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'R']
mean = numpy.mean(dfstats['Value'])
std = numpy.std(dfstats['Value'])
median = numpy.median(dfstats['Value'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 7.479302575107301
    Standard Deviation: 12.907750310407643
    Median: 4.0


## Green


```python
df[df['Color'] == 'G'][:25]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Price</th>
      <th>Title</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>49</th>
      <td>G</td>
      <td>119</td>
      <td>0.50</td>
      <td>acidic slime</td>
      <td>238.00</td>
    </tr>
    <tr>
      <th>91</th>
      <td>G</td>
      <td>83</td>
      <td>0.50</td>
      <td>reclamation sage</td>
      <td>166.00</td>
    </tr>
    <tr>
      <th>47</th>
      <td>G</td>
      <td>121</td>
      <td>0.75</td>
      <td>krosan grip</td>
      <td>161.33</td>
    </tr>
    <tr>
      <th>45</th>
      <td>G</td>
      <td>126</td>
      <td>0.79</td>
      <td>sakura-tribe elder</td>
      <td>159.49</td>
    </tr>
    <tr>
      <th>30</th>
      <td>G</td>
      <td>148</td>
      <td>1.03</td>
      <td>kodama's reach</td>
      <td>143.69</td>
    </tr>
    <tr>
      <th>123</th>
      <td>G</td>
      <td>70</td>
      <td>0.50</td>
      <td>rampant growth</td>
      <td>140.00</td>
    </tr>
    <tr>
      <th>137</th>
      <td>G</td>
      <td>66</td>
      <td>0.50</td>
      <td>terastodon</td>
      <td>132.00</td>
    </tr>
    <tr>
      <th>26</th>
      <td>G</td>
      <td>153</td>
      <td>1.19</td>
      <td>cultivate</td>
      <td>128.57</td>
    </tr>
    <tr>
      <th>175</th>
      <td>G</td>
      <td>56</td>
      <td>0.50</td>
      <td>elvish mystic</td>
      <td>112.00</td>
    </tr>
    <tr>
      <th>184</th>
      <td>G</td>
      <td>55</td>
      <td>0.50</td>
      <td>farhaven elf</td>
      <td>110.00</td>
    </tr>
    <tr>
      <th>213</th>
      <td>G</td>
      <td>50</td>
      <td>0.50</td>
      <td>soul of the harvest</td>
      <td>100.00</td>
    </tr>
    <tr>
      <th>225</th>
      <td>G</td>
      <td>49</td>
      <td>0.50</td>
      <td>rampaging baloths</td>
      <td>98.00</td>
    </tr>
    <tr>
      <th>239</th>
      <td>G</td>
      <td>48</td>
      <td>0.50</td>
      <td>shamanic revelation</td>
      <td>96.00</td>
    </tr>
    <tr>
      <th>269</th>
      <td>G</td>
      <td>45</td>
      <td>0.50</td>
      <td>zendikar resurgent</td>
      <td>90.00</td>
    </tr>
    <tr>
      <th>309</th>
      <td>G</td>
      <td>42</td>
      <td>0.50</td>
      <td>overwhelming stampede</td>
      <td>84.00</td>
    </tr>
    <tr>
      <th>105</th>
      <td>G</td>
      <td>76</td>
      <td>0.94</td>
      <td>yavimaya elder</td>
      <td>80.85</td>
    </tr>
    <tr>
      <th>355</th>
      <td>G</td>
      <td>38</td>
      <td>0.50</td>
      <td>harrow</td>
      <td>76.00</td>
    </tr>
    <tr>
      <th>386</th>
      <td>G</td>
      <td>36</td>
      <td>0.50</td>
      <td>overrun</td>
      <td>72.00</td>
    </tr>
    <tr>
      <th>403</th>
      <td>G</td>
      <td>35</td>
      <td>0.50</td>
      <td>genesis hydra</td>
      <td>70.00</td>
    </tr>
    <tr>
      <th>405</th>
      <td>G</td>
      <td>35</td>
      <td>0.50</td>
      <td>thunderfoot baloth</td>
      <td>70.00</td>
    </tr>
    <tr>
      <th>419</th>
      <td>G</td>
      <td>34</td>
      <td>0.50</td>
      <td>farseek</td>
      <td>68.00</td>
    </tr>
    <tr>
      <th>420</th>
      <td>G</td>
      <td>34</td>
      <td>0.50</td>
      <td>evolutionary leap</td>
      <td>68.00</td>
    </tr>
    <tr>
      <th>185</th>
      <td>G</td>
      <td>55</td>
      <td>0.81</td>
      <td>llanowar elves</td>
      <td>67.90</td>
    </tr>
    <tr>
      <th>437</th>
      <td>G</td>
      <td>33</td>
      <td>0.50</td>
      <td>archetype of endurance</td>
      <td>66.00</td>
    </tr>
    <tr>
      <th>461</th>
      <td>G</td>
      <td>32</td>
      <td>0.50</td>
      <td>karametra's acolyte</td>
      <td>64.00</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'G']
mean = numpy.mean(dfstats['Value'])
std = numpy.std(dfstats['Value'])
median = numpy.median(dfstats['Value'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 9.26203874538745
    Standard Deviation: 18.50107765372194
    Median: 4.0


## WU (Azorius)


```python
df[df['Color'] == 'WU'][:10]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Price</th>
      <th>Title</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>436</th>
      <td>WU</td>
      <td>33</td>
      <td>0.5</td>
      <td>render silent</td>
      <td>66.0</td>
    </tr>
    <tr>
      <th>462</th>
      <td>WU</td>
      <td>32</td>
      <td>0.5</td>
      <td>detention sphere</td>
      <td>64.0</td>
    </tr>
    <tr>
      <th>1393</th>
      <td>WU</td>
      <td>11</td>
      <td>0.5</td>
      <td>azorius charm</td>
      <td>22.0</td>
    </tr>
    <tr>
      <th>1517</th>
      <td>WU</td>
      <td>10</td>
      <td>0.5</td>
      <td>medomai the ageless</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>1649</th>
      <td>WU</td>
      <td>9</td>
      <td>0.5</td>
      <td>daxos of meletis</td>
      <td>18.0</td>
    </tr>
    <tr>
      <th>1616</th>
      <td>WU</td>
      <td>9</td>
      <td>0.5</td>
      <td>mistmeadow witch</td>
      <td>18.0</td>
    </tr>
    <tr>
      <th>1757</th>
      <td>WU</td>
      <td>8</td>
      <td>0.5</td>
      <td>lavinia of the tenth</td>
      <td>16.0</td>
    </tr>
    <tr>
      <th>2022</th>
      <td>WU</td>
      <td>7</td>
      <td>0.5</td>
      <td>isperia, supreme judge</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>2021</th>
      <td>WU</td>
      <td>7</td>
      <td>0.5</td>
      <td>righteous authority</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>2017</th>
      <td>WU</td>
      <td>7</td>
      <td>0.5</td>
      <td>azorius guildmage</td>
      <td>14.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'WU']
mean = numpy.mean(dfstats['Value'])
std = numpy.std(dfstats['Value'])
median = numpy.median(dfstats['Value'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 6.516756756756757
    Standard Deviation: 11.043546496081005
    Median: 2.0


## UB (Dimir)


```python
df[df['Color'] == 'UB'][:10]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Price</th>
      <th>Title</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>769</th>
      <td>UB</td>
      <td>20</td>
      <td>0.5</td>
      <td>notion thief</td>
      <td>40.0</td>
    </tr>
    <tr>
      <th>867</th>
      <td>UB</td>
      <td>18</td>
      <td>0.5</td>
      <td>evil twin</td>
      <td>36.0</td>
    </tr>
    <tr>
      <th>1367</th>
      <td>UB</td>
      <td>11</td>
      <td>0.5</td>
      <td>silumgar's command</td>
      <td>22.0</td>
    </tr>
    <tr>
      <th>1417</th>
      <td>UB</td>
      <td>11</td>
      <td>0.5</td>
      <td>dimir doppelganger</td>
      <td>22.0</td>
    </tr>
    <tr>
      <th>1523</th>
      <td>UB</td>
      <td>10</td>
      <td>0.5</td>
      <td>soul manipulation</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>1521</th>
      <td>UB</td>
      <td>10</td>
      <td>0.5</td>
      <td>psychic strike</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>1625</th>
      <td>UB</td>
      <td>9</td>
      <td>0.5</td>
      <td>mirko vosk, mind drinker</td>
      <td>18.0</td>
    </tr>
    <tr>
      <th>1789</th>
      <td>UB</td>
      <td>8</td>
      <td>0.5</td>
      <td>whispering madness</td>
      <td>16.0</td>
    </tr>
    <tr>
      <th>1817</th>
      <td>UB</td>
      <td>8</td>
      <td>0.5</td>
      <td>lim-dûl's vault</td>
      <td>16.0</td>
    </tr>
    <tr>
      <th>1993</th>
      <td>UB</td>
      <td>7</td>
      <td>0.5</td>
      <td>duskmantle guildmage</td>
      <td>14.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'UB']
mean = numpy.mean(dfstats['Value'])
std = numpy.std(dfstats['Value'])
median = numpy.median(dfstats['Value'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 5.798279569892472
    Standard Deviation: 7.003240513168282
    Median: 4.0


## BR (Rakdos)


```python
df[df['Color'] == 'BR'][:10]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Price</th>
      <th>Title</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>663</th>
      <td>BR</td>
      <td>23</td>
      <td>0.50</td>
      <td>rakdos charm</td>
      <td>46.00</td>
    </tr>
    <tr>
      <th>1127</th>
      <td>BR</td>
      <td>14</td>
      <td>0.50</td>
      <td>deathbringer thoctar</td>
      <td>28.00</td>
    </tr>
    <tr>
      <th>1291</th>
      <td>BR</td>
      <td>12</td>
      <td>0.50</td>
      <td>sire of insanity</td>
      <td>24.00</td>
    </tr>
    <tr>
      <th>1673</th>
      <td>BR</td>
      <td>9</td>
      <td>0.50</td>
      <td>rakdos's return</td>
      <td>18.00</td>
    </tr>
    <tr>
      <th>1684</th>
      <td>BR</td>
      <td>9</td>
      <td>0.50</td>
      <td>havoc festival</td>
      <td>18.00</td>
    </tr>
    <tr>
      <th>1676</th>
      <td>BR</td>
      <td>9</td>
      <td>0.50</td>
      <td>kolaghan, the storm's fury</td>
      <td>18.00</td>
    </tr>
    <tr>
      <th>1810</th>
      <td>BR</td>
      <td>8</td>
      <td>0.50</td>
      <td>torrent of souls</td>
      <td>16.00</td>
    </tr>
    <tr>
      <th>287</th>
      <td>BR</td>
      <td>44</td>
      <td>2.93</td>
      <td>terminate</td>
      <td>15.02</td>
    </tr>
    <tr>
      <th>363</th>
      <td>BR</td>
      <td>38</td>
      <td>2.92</td>
      <td>dreadbore</td>
      <td>13.01</td>
    </tr>
    <tr>
      <th>2256</th>
      <td>BR</td>
      <td>6</td>
      <td>0.50</td>
      <td>wrecking ball</td>
      <td>12.00</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'BR']
mean = numpy.mean(dfstats['Value'])
std = numpy.std(dfstats['Value'])
median = numpy.median(dfstats['Value'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 5.485897435897437
    Standard Deviation: 7.174526239321408
    Median: 2.115


## RG (Gruul)


```python
df[df['Color'] == 'RG'][:10]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Price</th>
      <th>Title</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>484</th>
      <td>RG</td>
      <td>31</td>
      <td>0.5</td>
      <td>hull breach</td>
      <td>62.0</td>
    </tr>
    <tr>
      <th>614</th>
      <td>RG</td>
      <td>25</td>
      <td>0.5</td>
      <td>fires of yavimaya</td>
      <td>50.0</td>
    </tr>
    <tr>
      <th>705</th>
      <td>RG</td>
      <td>22</td>
      <td>0.5</td>
      <td>mina and denn, wildborn</td>
      <td>44.0</td>
    </tr>
    <tr>
      <th>1040</th>
      <td>RG</td>
      <td>15</td>
      <td>0.5</td>
      <td>savage ventmaw</td>
      <td>30.0</td>
    </tr>
    <tr>
      <th>1227</th>
      <td>RG</td>
      <td>13</td>
      <td>0.5</td>
      <td>atarka, world render</td>
      <td>26.0</td>
    </tr>
    <tr>
      <th>1405</th>
      <td>RG</td>
      <td>11</td>
      <td>0.5</td>
      <td>spellbreaker behemoth</td>
      <td>22.0</td>
    </tr>
    <tr>
      <th>1487</th>
      <td>RG</td>
      <td>10</td>
      <td>0.5</td>
      <td>ruric thar, the unbowed</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>2041</th>
      <td>RG</td>
      <td>7</td>
      <td>0.5</td>
      <td>zhur-taa druid</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>2037</th>
      <td>RG</td>
      <td>7</td>
      <td>0.5</td>
      <td>clan defiance</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>1957</th>
      <td>RG</td>
      <td>7</td>
      <td>0.5</td>
      <td>rubblehulk</td>
      <td>14.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'RG']
mean = numpy.mean(dfstats['Value'])
std = numpy.std(dfstats['Value'])
median = numpy.median(dfstats['Value'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 7.308194444444443
    Standard Deviation: 11.1266836836285
    Median: 2.88


## WB (Orzhov)


```python
df[df['Color'] == 'WB'][:10]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Price</th>
      <th>Title</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>385</th>
      <td>WB</td>
      <td>36</td>
      <td>0.50</td>
      <td>utter end</td>
      <td>72.0</td>
    </tr>
    <tr>
      <th>407</th>
      <td>WB</td>
      <td>35</td>
      <td>0.50</td>
      <td>merciless eviction</td>
      <td>70.0</td>
    </tr>
    <tr>
      <th>409</th>
      <td>WB</td>
      <td>35</td>
      <td>0.79</td>
      <td>mortify</td>
      <td>44.3</td>
    </tr>
    <tr>
      <th>868</th>
      <td>WB</td>
      <td>18</td>
      <td>0.50</td>
      <td>ashen rider</td>
      <td>36.0</td>
    </tr>
    <tr>
      <th>1197</th>
      <td>WB</td>
      <td>13</td>
      <td>0.50</td>
      <td>debt to the deathless</td>
      <td>26.0</td>
    </tr>
    <tr>
      <th>1195</th>
      <td>WB</td>
      <td>13</td>
      <td>0.50</td>
      <td>ayli, eternal pilgrim</td>
      <td>26.0</td>
    </tr>
    <tr>
      <th>1269</th>
      <td>WB</td>
      <td>12</td>
      <td>0.50</td>
      <td>vizkopa guildmage</td>
      <td>24.0</td>
    </tr>
    <tr>
      <th>1407</th>
      <td>WB</td>
      <td>11</td>
      <td>0.50</td>
      <td>teysa, envoy of ghosts</td>
      <td>22.0</td>
    </tr>
    <tr>
      <th>1413</th>
      <td>WB</td>
      <td>11</td>
      <td>0.50</td>
      <td>obzedat's aid</td>
      <td>22.0</td>
    </tr>
    <tr>
      <th>1641</th>
      <td>WB</td>
      <td>9</td>
      <td>0.50</td>
      <td>treasury thrull</td>
      <td>18.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'WB']
mean = numpy.mean(dfstats['Value'])
std = numpy.std(dfstats['Value'])
median = numpy.median(dfstats['Value'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 10.04535714285714
    Standard Deviation: 15.064585239689725
    Median: 4.0


## UR (Izzet)


```python
df[df['Color'] == 'UR'][:10]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Price</th>
      <th>Title</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>745</th>
      <td>UR</td>
      <td>21</td>
      <td>0.5</td>
      <td>counterflux</td>
      <td>42.0</td>
    </tr>
    <tr>
      <th>1304</th>
      <td>UR</td>
      <td>12</td>
      <td>0.5</td>
      <td>izzet charm</td>
      <td>24.0</td>
    </tr>
    <tr>
      <th>1397</th>
      <td>UR</td>
      <td>11</td>
      <td>0.5</td>
      <td>dack's duplicate</td>
      <td>22.0</td>
    </tr>
    <tr>
      <th>1408</th>
      <td>UR</td>
      <td>11</td>
      <td>0.5</td>
      <td>goblin electromancer</td>
      <td>22.0</td>
    </tr>
    <tr>
      <th>1411</th>
      <td>UR</td>
      <td>11</td>
      <td>0.5</td>
      <td>hypersonic dragon</td>
      <td>22.0</td>
    </tr>
    <tr>
      <th>1650</th>
      <td>UR</td>
      <td>9</td>
      <td>0.5</td>
      <td>jori en, ruin diver</td>
      <td>18.0</td>
    </tr>
    <tr>
      <th>1606</th>
      <td>UR</td>
      <td>9</td>
      <td>0.5</td>
      <td>niv-mizzet, dracogenius</td>
      <td>18.0</td>
    </tr>
    <tr>
      <th>1612</th>
      <td>UR</td>
      <td>9</td>
      <td>0.5</td>
      <td>mercurial chemister</td>
      <td>18.0</td>
    </tr>
    <tr>
      <th>1622</th>
      <td>UR</td>
      <td>9</td>
      <td>0.5</td>
      <td>melek, izzet paragon</td>
      <td>18.0</td>
    </tr>
    <tr>
      <th>2135</th>
      <td>UR</td>
      <td>6</td>
      <td>0.5</td>
      <td>epic experiment</td>
      <td>12.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'UR']
mean = numpy.mean(dfstats['Value'])
std = numpy.std(dfstats['Value'])
median = numpy.median(dfstats['Value'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 7.2761538461538455
    Standard Deviation: 8.271511384971669
    Median: 4.0


## BG (Golgari)


```python
df[df['Color'] == 'BG'][:10]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Price</th>
      <th>Title</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>344</th>
      <td>BG</td>
      <td>39</td>
      <td>0.5</td>
      <td>putrefy</td>
      <td>78.0</td>
    </tr>
    <tr>
      <th>1059</th>
      <td>BG</td>
      <td>15</td>
      <td>0.5</td>
      <td>deathreap ritual</td>
      <td>30.0</td>
    </tr>
    <tr>
      <th>1063</th>
      <td>BG</td>
      <td>15</td>
      <td>0.5</td>
      <td>golgari charm</td>
      <td>30.0</td>
    </tr>
    <tr>
      <th>1137</th>
      <td>BG</td>
      <td>14</td>
      <td>0.5</td>
      <td>reaper of the wilds</td>
      <td>28.0</td>
    </tr>
    <tr>
      <th>1128</th>
      <td>BG</td>
      <td>14</td>
      <td>0.5</td>
      <td>jarad's orders</td>
      <td>28.0</td>
    </tr>
    <tr>
      <th>1661</th>
      <td>BG</td>
      <td>9</td>
      <td>0.5</td>
      <td>corpsejack menace</td>
      <td>18.0</td>
    </tr>
    <tr>
      <th>1656</th>
      <td>BG</td>
      <td>9</td>
      <td>0.5</td>
      <td>gaze of granite</td>
      <td>18.0</td>
    </tr>
    <tr>
      <th>1840</th>
      <td>BG</td>
      <td>8</td>
      <td>0.5</td>
      <td>grisly salvage</td>
      <td>16.0</td>
    </tr>
    <tr>
      <th>2046</th>
      <td>BG</td>
      <td>7</td>
      <td>0.5</td>
      <td>korozda guildmage</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>2011</th>
      <td>BG</td>
      <td>7</td>
      <td>0.5</td>
      <td>varolz, the scar-striped</td>
      <td>14.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'BG']
mean = numpy.mean(dfstats['Value'])
std = numpy.std(dfstats['Value'])
median = numpy.median(dfstats['Value'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 7.728524590163937
    Standard Deviation: 11.717977618279955
    Median: 3.63


## RW (Boros)


```python
df[df['Color'] == 'WR'][:10]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Price</th>
      <th>Title</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>671</th>
      <td>WR</td>
      <td>23</td>
      <td>0.5</td>
      <td>assemble the legion</td>
      <td>46.0</td>
    </tr>
    <tr>
      <th>1278</th>
      <td>WR</td>
      <td>12</td>
      <td>0.5</td>
      <td>deflecting palm</td>
      <td>24.0</td>
    </tr>
    <tr>
      <th>1284</th>
      <td>WR</td>
      <td>12</td>
      <td>0.5</td>
      <td>legion's initiative</td>
      <td>24.0</td>
    </tr>
    <tr>
      <th>1526</th>
      <td>WR</td>
      <td>10</td>
      <td>0.5</td>
      <td>tajic, blade of the legion</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>1486</th>
      <td>WR</td>
      <td>10</td>
      <td>0.5</td>
      <td>aurelia's fury</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>1490</th>
      <td>WR</td>
      <td>10</td>
      <td>0.5</td>
      <td>master warcraft</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>1651</th>
      <td>WR</td>
      <td>9</td>
      <td>0.5</td>
      <td>firemane avenger</td>
      <td>18.0</td>
    </tr>
    <tr>
      <th>1683</th>
      <td>WR</td>
      <td>9</td>
      <td>0.5</td>
      <td>glory of warfare</td>
      <td>18.0</td>
    </tr>
    <tr>
      <th>1940</th>
      <td>WR</td>
      <td>7</td>
      <td>0.5</td>
      <td>nobilis of war</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>2161</th>
      <td>WR</td>
      <td>6</td>
      <td>0.5</td>
      <td>agrus kos, wojek veteran</td>
      <td>12.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'WR']
mean = numpy.mean(dfstats['Value'])
std = numpy.std(dfstats['Value'])
median = numpy.median(dfstats['Value'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 6.165138888888888
    Standard Deviation: 7.3488360743301735
    Median: 4.0


## GU (Simic)


```python
df[df['Color'] == 'UG'][:10]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Price</th>
      <th>Title</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>699</th>
      <td>UG</td>
      <td>22</td>
      <td>0.50</td>
      <td>coiling oracle</td>
      <td>44.00</td>
    </tr>
    <tr>
      <th>776</th>
      <td>UG</td>
      <td>20</td>
      <td>0.50</td>
      <td>altered ego</td>
      <td>40.00</td>
    </tr>
    <tr>
      <th>871</th>
      <td>UG</td>
      <td>18</td>
      <td>0.50</td>
      <td>prime speaker zegana</td>
      <td>36.00</td>
    </tr>
    <tr>
      <th>978</th>
      <td>UG</td>
      <td>16</td>
      <td>0.50</td>
      <td>plasm capture</td>
      <td>32.00</td>
    </tr>
    <tr>
      <th>1047</th>
      <td>UG</td>
      <td>15</td>
      <td>0.50</td>
      <td>trygon predator</td>
      <td>30.00</td>
    </tr>
    <tr>
      <th>410</th>
      <td>UG</td>
      <td>35</td>
      <td>1.58</td>
      <td>prophet of kruphix</td>
      <td>22.15</td>
    </tr>
    <tr>
      <th>1388</th>
      <td>UG</td>
      <td>11</td>
      <td>0.50</td>
      <td>urban evolution</td>
      <td>22.00</td>
    </tr>
    <tr>
      <th>1594</th>
      <td>UG</td>
      <td>9</td>
      <td>0.50</td>
      <td>kiora's follower</td>
      <td>18.00</td>
    </tr>
    <tr>
      <th>1603</th>
      <td>UG</td>
      <td>9</td>
      <td>0.50</td>
      <td>bring to light</td>
      <td>18.00</td>
    </tr>
    <tr>
      <th>1771</th>
      <td>UG</td>
      <td>8</td>
      <td>0.50</td>
      <td>edric, spymaster of trest</td>
      <td>16.00</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = df[df['Color'] == 'UG']
mean = numpy.mean(dfstats['Value'])
std = numpy.std(dfstats['Value'])
median = numpy.median(dfstats['Value'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 9.469
    Standard Deviation: 10.699574243865968
    Median: 4.96


## Other
Three or more colors.


```python
mask = (df['Color'].str.len() >= 3)
dfother = df.loc[mask]
dfother[:10]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Color</th>
      <th>Commanders</th>
      <th>Price</th>
      <th>Title</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1305</th>
      <td>UBR</td>
      <td>12</td>
      <td>0.50</td>
      <td>cruel ultimatum</td>
      <td>24.00</td>
    </tr>
    <tr>
      <th>1419</th>
      <td>UBR</td>
      <td>11</td>
      <td>0.50</td>
      <td>crosis's charm</td>
      <td>22.00</td>
    </tr>
    <tr>
      <th>1638</th>
      <td>WUB</td>
      <td>9</td>
      <td>0.50</td>
      <td>magister sphinx</td>
      <td>18.00</td>
    </tr>
    <tr>
      <th>1783</th>
      <td>WRG</td>
      <td>8</td>
      <td>0.50</td>
      <td>naya charm</td>
      <td>16.00</td>
    </tr>
    <tr>
      <th>1784</th>
      <td>WRG</td>
      <td>8</td>
      <td>0.50</td>
      <td>titanic ultimatum</td>
      <td>16.00</td>
    </tr>
    <tr>
      <th>2028</th>
      <td>URG</td>
      <td>7</td>
      <td>0.50</td>
      <td>temur ascendancy</td>
      <td>14.00</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>UBR</td>
      <td>7</td>
      <td>0.50</td>
      <td>slave of bolas</td>
      <td>14.00</td>
    </tr>
    <tr>
      <th>2166</th>
      <td>UBR</td>
      <td>6</td>
      <td>0.50</td>
      <td>grixis charm</td>
      <td>12.00</td>
    </tr>
    <tr>
      <th>2253</th>
      <td>URG</td>
      <td>6</td>
      <td>0.50</td>
      <td>surrak dragonclaw</td>
      <td>12.00</td>
    </tr>
    <tr>
      <th>1537</th>
      <td>WUG</td>
      <td>10</td>
      <td>0.91</td>
      <td>bant charm</td>
      <td>10.99</td>
    </tr>
  </tbody>
</table>
</div>




```python
dfstats = dfother
mean = numpy.mean(dfstats['Value'])
std = numpy.std(dfstats['Value'])
median = numpy.median(dfstats['Value'])

print("Mean: " + str(mean))
print("Standard Deviation: " + str(std))
print("Median: " + str(median))
```

    Mean: 3.2948502994011957
    Standard Deviation: 4.176357014399215
    Median: 2.0
