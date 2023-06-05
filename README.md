# Azure
A common perception In the Tekhelet-tying community is that the blue strings are more vulnerable to the elements, despite their pronounced elegence and cost.
See [this claim](https://twitter.com/shalom_kahana/status/1631060881772609539?t=ZT7xLtskc37uN5zoCoaXeA&s=19) in twitter, and the strong sense of agreement in the comments.  
Therefore, I took to myself to verify this claim.
For that purpose I undid an old Tzitzit of mine, and measured each strand.
My Tzitzit's 16 strings were comprised equally of white and blue, in accordance to the tradition of the medieval scholar Rashi, and his disciples, the Tosafot.

![image](https://user-images.githubusercontent.com/131248454/233946185-c152daad-5cf9-433f-9909-5c33e3042d80.png)



I then ran a paired t-test to determine whether the differences should be attributed to the coloring, and whether other factors could explain the difference. In the end, I coded it all in Python.

    import pandas as pd
    import numpy as np
    import matplotlib.pyplot as plt
    from scipy.stats import ttest_rel

After executing the necessary imports, I created the dataframe:


    azure = [['white',150,142,'front','left','yed_21'],
    ['white',120,103,'front','left','yed_21'],
    ['blue',120,102,'front','left','yed_21'],
    ['blue',150,89,'front','left','yed_21'],
    ['white',120,106,'front','right','yed_21'],
    ['blue',150,135,'front','right','yed_21'],
    ['blue',120,72,'front','right','yed_21'],
    ['white',150,134,'front','right','yed_21'],
    ['white',150,137,'back','left','yed_21'],
    ['white',120,109,'back','left','yed_21'],
    ['blue',150,131,'back','left','yed_21'],
    ['blue',120,88,'back','left','yed_21'],
    ['blue',150,98,'back','right','yed_21'],
    ['white',150,132,'back','right','yed_21'],
    ['white',120,102,'back','right','yed_21'],
    ['blue',120,84,'back','right','yed_21']]
    
    df = pd.DataFrame(azure, columns=['color', 'orig_ln','pres_ln','f/b','r/l','cloth']) 
    df
    df.info()
    
I calculated the length that each string lost over the course of 2 years, and slipped in the following column:


    df.insert(3, "difference",df['orig_ln']-df['pres_ln'], True)
    df
![image](https://user-images.githubusercontent.com/131248454/233931497-cd2573b2-0715-47da-b7ff-617e55ff4f3b.png)

I pivoted the table for ease of comparison:


    p_table = pd.pivot_table(df, index = ['f/b', 'r/l','orig_ln','color'],values=['difference'])
    p_table
![צילום מסך 2023-04-24 104904](https://user-images.githubusercontent.com/131248454/233932734-129d09f9-ae54-4770-a9bc-05110d5911b9.png)

And split the data into 2 symetrical groups by each independent parameter.


    group_blue = df1[df1['color']=='blue']
    group_white = df1[df1['color']=='white']
    group_f = df1[df1['f/b']=='front']
    group_b = df1[df1['f/b']=='back']
    group_r = df1[df1['r/l']=='right']
    group_l = df1[df1['r/l']=='left']
    group_150 = df1[df1['orig_ln']==150]
    group_120 = df1[df1['orig_ln']==120]
    
 Having the strings fall neatly into 2 even, identical groups, regardless of the parameter, I ruled that the proper test for statistical significance was a paired sample t-test.
 
 
    dif_color = ttest_rel(group_blue['difference'], group_white['difference'])
    dif_position_fb = ttest_rel(group_f['difference'], group_b['difference'])
    dif_position_rl = ttest_rel(group_r['difference'], group_l['difference'])
    dif_len = ttest_rel(group_150['difference'], group_120['difference'])
    print('white',' ','blue',' ',dif_color)
    print('front',' ','back',' ',dif_position_fb)
    print('right',' ','left',' ',dif_position_rl)
    print('150',' ','120',' ',dif_len)

The color shows to be an overwhelmingly significant factor in the current length of the string, in stark comparison the other factors tested.
The contrast stands out even when taking into account the small sample size:

>white   blue   TtestResult(statistic=3.914522024707396, **pvalue=0.00578966336216682**, df=7)

>front   back   TtestResult(statistic=-0.03094287251686777, pvalue=0.9761788614811876, df=7)

>right   left   TtestResult(statistic=0.5376162432255781, pvalue=0.6075017336083551, df=7)

>150   120   TtestResult(statistic=0.1103354568734741, pvalue=0.9152400725029919, df=7)

Visuals to support the claim are always welcome, so I added a couple of rows:


    pivoted = df.pivot(index=['f/b', 'r/l','orig_ln'], columns="color", values="difference")
    pivoted.insert(2, "hefresh",pivoted['blue']-pivoted['white'], True)
    ax = pivoted.plot.bar(y=['blue', 'white'], color=['blue', 'white'])
    ax.set_facecolor((0.8,0.8,0.8))
    fig = ax.get_figure()
    fig.patch.set_facecolor((0.9,0.9,0.9))
    ax.set_xlabel('')
    plt.show()
    

This chart shows the length of strings lost, in paires identical in position and original length

![image](https://user-images.githubusercontent.com/131248454/233987897-909e6e6b-5d72-4fde-804b-8ee406b85a8f.png)


    pivoted1 = df.pivot(index=['f/b', 'r/l','orig_ln'], columns="color", values="pres_ln")
    ax1 = pivoted1.plot.bar(y=['blue', 'white'], color=['blue', 'white'])
    ax1.set_facecolor((0.8,0.8,0.8))
    fig1 = ax1.get_figure()
    fig1.patch.set_facecolor((0.9,0.9,0.9))
    ax1.set_xlabel('')
    plt.show()
    



This chart shows the same pairs, this time expressed in terms of string remaining.


![image](https://user-images.githubusercontent.com/131248454/233988101-1f5b5c7b-1ff9-4ff6-8dd6-5d65c61ab332.png)

All figures are given in CM.



