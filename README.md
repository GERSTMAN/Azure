# Azure
A common perception In the Tekhelet-tying community is that the blue strings are more vulnerable to the elements, despite their pronounced elegence and cost.
See [this claim](https://twitter.com/shalom_kahana/status/1631060881772609539?t=ZT7xLtskc37uN5zoCoaXeA&s=19) made by a user in twitter, and the strong sense of agreement that the tweet raised.  
Therefore, I took to myself to verify this claim.
For that purpose I undid an old Tzitzit of mine, and measured every single strand.
I then ran a t-test to determine whether the differences should be attributed to the shade of the strings, or to positional causes.

    import pandas as pd
    import numpy as np
    import datetime
    import matplotlib.pyplot as plt
    from scipy.stats import ttest_rel

After executing the necessary imports, I created the following dataframe:


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
    
I calculated the loss of length for each string, and slipped the following column:


    df.insert(3, "difference",df['orig_ln']-df['pres_ln'], True)

