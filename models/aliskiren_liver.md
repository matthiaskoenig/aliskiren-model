# model: aliskiren_liver
Autogenerated ODE System from SBML with [sbmlutils](https://github.com/matthiaskoenig/sbmlutils).
```
time: [min]
substance: [mmol]
extent: [mmol]
volume: [l]
area: [m^2]
length: [m]
```

## Parameters `p`
```
ALIEX_k = 0.0001  # [1/min] rate for aliskiren export in bile  
ALIIM_Km_ali = 0.1  # [mmol/l] Km aliskiren import  
ALIIM_Vmax = 100.0  # [mmol/min/l] Vmax aliskiren import  
ALM_Km_ali = 0.1  # [mmol/l] Km aliskiren metabolism  
ALM_Vmax = 1.0  # [mmol/min/l] Vmax aliskiren metabolism  
Vapical = nan  # [m^2] apical membrane  
Vbi = 1.0  # [l] bile  
Vext = 1.5  # [l] plasma  
Vli = 1.5  # [l] liver  
Vlumen = 1.15425  # [l] intestinal lumen (inner part of intestine)  
Vmem = nan  # [m^2] plasma membrane  
f_cyp3a4 = 1.0  # [-] scaling factor cyp3a4 activity  
```

## Initial conditions `x0`
```
ali = 0.0  # [mmol/l] aliskiren (liver) in Vli  
ali_bi = 0.0  # [mmol] aliskiren (bile) in Vbi  
ali_ext = 0.0  # [mmol/l] aliskiren (plasma) in Vext  
ali_lumen = 0.0  # [mmol/l] aliskiren (lumen) in Vlumen  
alm = 0.0  # [mmol/l] aliskiren metabolite (liver) in Vli  
```

## ODE system
```
# y
ALIEX = ALIEX_k * Vli * ali  # [mmol/min] aliskiren bile export  
ALIIM = (ALIIM_Vmax / ALIIM_Km_ali) * Vli * (ali_ext - ali) / (1 + ali_ext / ALIIM_Km_ali + ali / ALIIM_Km_ali)  # [mmol/min] aliskiren import (ALIIM)  
ALM = f_cyp3a4 * ALM_Vmax * Vli * ali / (ali + ALM_Km_ali)  # [mmol/min] aliskiren metabolism via CYP3A4  
ALIEHC = ALIEX  # [mmol/min] aliskiren enterohepatic circulation  

# odes
d ali/dt = ALIIM / Vli - ALM / Vli - ALIEX / Vli  # [mmol/l/] aliskiren (liver)  
d ali_bi/dt = ALIEX - ALIEHC  # [mmol/] aliskiren (bile)  
d ali_ext/dt = -ALIIM / Vext  # [mmol/l/] aliskiren (plasma)  
d ali_lumen/dt = ALIEHC / Vlumen  # [mmol/l/] aliskiren (lumen)  
d alm/dt = ALM / Vli  # [mmol/l/] aliskiren metabolite (liver)  
```