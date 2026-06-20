# خب اینم از پروژه محاسبه جرم مولی خودم :)
# زهرا - بهار1405
# راستش اولش سخت بود ولی وقتی منطقش رو فهمیدم راحت شد

# جرم اتمی عناصری که توی دبیرستان باهاشون کار داریم
# بقیه رو بعدا اضافه میکنم
jerm_atomi = {
    'H': 1.008, 'C': 12.011, 'N': 14.007, 'O': 15.999,
    'F': 18.998, 'Na': 22.990, 'Mg': 24.305, 'Al': 26.982,
    'P': 30.974, 'S': 32.06, 'Cl': 35.45, 'K': 39.098,
    'Ca': 40.078, 'Fe': 55.845, 'Cu': 63.546, 'Zn': 65.38,
    'Br': 79.904, 'Ag': 107.87, 'I': 126.90, 'Ba': 137.33,
    'Au': 196.97, 'Hg': 200.59, 'Pb': 207.2,
}

def baz_kon_parantez(formul):
    """
    این تابع فرمول رو میگیره و عناصرش رو میشمره
    مثلا Ca(OH)2 رو میکنه {'Ca':1, 'O':2, 'H':2}
    کلی وقتمو گرفت این بخش :|
    """
    # اول فرمول رو توی یه لیست میذارم تا بتونم دونه دونه بررسیش کنم
    tak_tak = []
    i = 0
    while i < len(formul):
        # پرانتز باز یا بسته
        if formul[i] == '(' or formul[i] == ')':
            tak_tak.append(formul[i])
            i = i + 1
        # حرف بزرگ یعنی عنصر جدید
        elif formul[i].isupper():
            nam = formul[i]
            i = i + 1
            # حروف کوچیک بعدش رو هم میخونم (مثلا a توی Ca)
            while i < len(formul) and formul[i].islower():
                nam = nam + formul[i]
                i = i + 1
            tak_tak.append(nam)
        # عدد یعنی تعداد
        elif formul[i].isdigit():
            adad = ''
            while i < len(formul) and formul[i].isdigit():
                adad = adad + formul[i]
                i = i + 1
            tak_tak.append(adad)
        else:
            i = i + 1
    
    # حالا باید این لیست رو تجزیه کنم
    # ایده از اینجا اومد: هرجا به پرانتز باز خوردم برم تو دلش
    natije = {}
    i = 0
    while i < len(tak_tak):
        # رسیدیم به پرانتز باز، پس باید بریم تو
        if tak_tak[i] == '(':
            # یه دونه میرم جلو که برسم به اولین عنصر داخل پرانتز
            i = i + 1
            # یه لیست واسه نگه داری عناصر داخل پرانتز
            tooye_parantez = {}
            while i < len(tak_tak) and tak_tak[i] != ')':
                # داخل پرانتز هم میتونه عنصر باشه
                if tak_tak[i][0].isupper():
                    el = tak_tak[i]
                    i = i + 1
                    meghdar = 1
                    if i < len(tak_tak) and tak_tak[i].isdigit():
                        meghdar = int(tak_tak[i])
                        i = i + 1
                    tooye_parantez[el] = tooye_parantez.get(el, 0) + meghdar
                else:
                    i = i + 1
            # رسیدم به )
            if i < len(tak_tak) and tak_tak[i] == ')':
                i = i + 1
                # حالا عدد بعد از پرانتز مهمه
                zarib = 1
                if i < len(tak_tak) and tak_tak[i].isdigit():
                    zarib = int(tak_tak[i])
                    i = i + 1
                # ضرب کردن همه چیز توی zarib
                for el, megh in tooye_parantez.items():
                    natije[el] = natije.get(el, 0) + megh * zarib
        # عنصر معمولی خارج از پرانتز
        elif tak_tak[i][0].isupper():
            el = tak_tak[i]
            i = i + 1
            meghdar = 1
            if i < len(tak_tak) and tak_tak[i].isdigit():
                meghdar = int(tak_tak[i])
                i = i + 1
            natije[el] = natije.get(el, 0) + meghdar
        else:
            i = i + 1
    
    return natije

def mohasebe_jerm_moli(formul):
    # فرمول رو تجزیه کن
    try:
        elementha = baz_kon_parantez(formul)
    except:
        print("یه جای کار میلنگه! فرمول رو چک کن")
        return None
    
    jerm_kol = 0
    print(f"\nفرمول: {formul}")
    print("-" * 35)
    
    for nam, tedad in elementha.items():
        if nam in jerm_atomi:
            jerm_har_eleman = jerm_atomi[nam] * tedad
            jerm_kol = jerm_kol + jerm_har_eleman
            print(f"{nam}: {tedad} × {jerm_atomi[nam]} = {jerm_har_eleman:.2f}")
        else:
            print(f"عنصر {nam} رو نمیشناسم :|")
            return None
    
    print("-" * 35)
    return jerm_kol

# ============ اجرای برنامه ============
print("=" * 40)
print("🧪 ماشین حساب جرم مولی - ساخته زهرا")
print("=" * 40)
print("مثال: H2O , Ca(OH)2 , C6H12O6")
print("بنویس exit تا خارج بشی\n")

while True:
    vorodi = input("فرمول رو وارد کن: ").strip()
    
    if vorodi == 'exit':
        print("مرسی که استفاده کردی 😊")
        break
    
    if vorodi == '':
        continue
    
    javab = mohasebe_jerm_moli(vorodi)
    
    if javab:
        print(f"جرم مولی نهایی: {javab:.2f} g/mol\n")
    else:
        print("دوباره تلاش کن!\n")
