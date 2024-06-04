Hydra, nüfuz testçilərinə və etik hakerlərə şəbəkə xidmətləri parollarını sındırmağa kömək edən bir parol sayma vasitəsidir.

Hydra 50-dən çox protokol üçün sürətli lüğət hücumları edə bilər. Buraya telnet, FTP, HTTP, HTTPS, SMB, verilənlər bazası və bir sıra digər xidmətlər daxildir.

Hydra, "Hacker' s Choice"haker qrupu tərəfindən hazırlanmışdır. Hydra ilk dəfə 2000-ci ildə şəbəkə giriş xidmətlərinə hücumları necə həyata keçirə biləcəyinizi nümayiş etdirən bir konsepsiya yoxlama vasitəsi olaraq buraxıldı.

Hydra eyni zamanda paralel bir giriş hackidir. Bu o deməkdir ki, bir neçə paralel bağlantınız ola bilər. Ardıcıl hackdən fərqli olaraq, bu, parolun sındırılması üçün lazım olan vaxtı azaldır.

Son məqaləmdə John The Ripper adlı başqa bir hack alətini təsvir etdim. John və Hydra kobud güc vasitələrindən istifadə etsələr də, Hydra onlayn işləyərkən John oflayn işləyir.

Bu yazıda Hydra ' nın necə işlədiyini araşdıracağıq və sonra bəzi real istifadə nümunələrini nəzərdən keçirəcəyik.

    Qeyd: bütün məqalələrim məlumat məqsədləri üçündür. Onları qanunsuz istifadə edirsinizsə və probleminiz varsa, məsuliyyət daşımıram. Sistemi taramadan / sındırmadan / işlətmədən əvvəl həmişə sahibindən icazə istəyin.

Hydra-nı necə quraşdırmaq olar

Hydra, Kali Linux və Parrot OS-nin əvvəlcədən quraşdırılmış versiyaları ilə gəlir. Beləliklə, onlardan birini istifadə edirsinizsə, dərhal Hydra ilə işləməyə başlaya bilərsiniz.

Ubuntu-da onu apt paket meneceri ilə quraşdıra bilərsiniz:

    apt install hydra

Mac-da Hydra-nı Homebrev bölməsində tapa bilərsiniz:

    brew install hydra

Pəncərələrdən istifadə edirsinizsə, virtual qutudan istifadə etməyi və Linux-u quraşdırmağı məsləhət görürəm. Şəxsən mən peşəkar nüfuz testçisi olmaq istəyirsinizsə, pəncərələrdən istifadə etməyi məsləhət görmürəm.

Hydra ilə necə işləmək olar

Hydra ilə necə işləyəcəyimizi nəzərdən keçirək. Hydra-nın istifadəçi adlarını və şifrələrini sındırmaq üçün təqdim etdiyi ümumi formatları və seçimləri nəzərdən keçirəcəyik. Buraya tək bir istifadəçi adı/Şifrə hücumları, şifrələrin yayılması və lüğət hücumları daxildir.

Hydra quraşdırılmışsa, help əmri ilə aşağıdakı kimi başlaya bilərsiniz:

    hydra -h

Bu, Hydra ilə işləyərkən istinad kimi istifadə edə biləcəyiniz bayraqların və seçimlərin siyahısını verəcəkdir.

Hydra ilə bir istifadəçi adı və şifrə ilə hücumu necə həyata keçirmək olar

Sadə bir hücumla başlayaq. Sistemdə olmasını gözlədiyimiz bir istifadəçi adımız və şifrəmiz varsa, onları Hydra ilə sınaqdan keçirə bilərik.

Budur sintaksis:

    hydra -l <istifadəçi adı> -p <parol> <server> <xidmət>

Fərz edək ki, 10.10.137.76-da "Molly" adlı və "butterfly" şifrəsi olan bir istifadəçimiz var. SSH üçün etimadnaməni yoxlamaq üçün Hydra-dan necə istifadə edə bilərik:

    hydra -l molly -p butterfly 10.10.137.76 ssh

Hydra ilə parol hücumunu necə yerinə yetirmək olar

Birinin istifadə etdiyi şifrəni bilsək, amma kim olduğuna əmin olmasaq nə olar? İstifadəçi adını təyin etmək üçün parol hücumundan istifadə edə bilərik.

Şifrə püskürtmə hücumu, bir parol istifadə etdiyimiz və birdən çox istifadəçiyə tətbiq etdiyimiz zamandır. Kimsə bu paroldan istifadə edirsə, Hydra bizim üçün uyğun olanı tapacaq.

Bu hücum sistemdəki istifadəçilərin siyahısını bildiyimizi göstərir. Bu nümunə üçün istifadəçilər adlı bir fayl yaradacağıq.aşağıdakı istifadəçilər üçün txt:

    root
    admin
    user
    molly
    steve
    richard

İndi "butterfly" parolunun kim olduğunu yoxlayacağıq. Hydra ilə parol çiləmə üsulu ilə hücumu necə həyata keçirə bilərik.

    hydra -L users.txt -p butterfly 10.10.137.76 ssh

İstifadəçilərdən hər hansı biri müəyyən bir şifrə ilə uyğun gəlsə, aşağıdakılara bənzər bir nəticə əldə edəcəyik. Siz həmçinin qeyd etməlisiniz ki, biz -l əvəzinə -L bayrağından istifadə etdik. -l tək istifadəçi adı üçün, -L isə istifadəçi adlarının siyahısı üçündür. İstifadəçilərdən hər hansı biri müəyyən bir şifrə ilə uyğun gəlsə, aşağıdakılara bənzər bir nəticə əldə edəcəyik. Siz həmçinin qeyd etməlisiniz ki, biz -l əvəzinə -L bayrağından istifadə etdik. -l tək istifadəçi adı üçün,-L isə istifadəçi adlarının siyahısı üçündür.

Hydra ilə lüğət hücumunu necə yerinə yetirmək olar

Lüğət hücumunu necə həyata keçirəcəyimizə baxaq. Həqiqi ssenarilərdə bunun üçün mütəmadi olaraq Hydra istifadə edəcəyik.

Lüğət hücumu bir və ya daha çox istifadəçi adımız olduqda və Hydra-ya parol siyahısı təqdim etdiyimiz zamandır. Hydra daha sonra siyahıdakı hər bir istifadəçidəki bütün bu parolları yoxlayır.

Bu nümunədə, istifadəçilər faylı ilə birlikdə Rockyou sözlərinin siyahısını istifadə edəcəyəm.əvvəlki hücumda yaratdığımız txt. Kali Linux istifadə edirsinizsə, RockYou sözlərinin siyahısını /usr/share/sözlər/rockyou altında tapa bilərsiniz.txt.

Budur lüğət hücumu əmri:

    hydra -L users.txt -P /usr/share/wordlists/rockyou.txt 1010.137.76 ssh

Bu hücum uğurlu olarsa, digər iki komandaya bənzər bir nəticə görəcəyik. Hydra, bütün matçlar üçün uğurlu istifadəçi adı və parol birləşmələrini yaşıllaşdıracaq.

Hydra-da detal və ayıklama bayraqlarından necə istifadə olunur

Hydra böyük kobud hücumlar edərkən çox sakit işləyə bilər. Hydra-nın ondan gözlənilənləri yerinə yetirdiyinə əmin olmaq lazımdırsa, istifadə edə biləcəyimiz iki bayraq var.

Ətraflı bayraq (- v) bizə hər bir istifadəçi adı və şifrə birləşməsi üçün giriş cəhdini göstərəcəkdir. Bir çox kombinasiyadan keçmək lazım olduqda Bu bir az çətin ola bilər, amma ehtiyacınız varsa, detal bayrağından istifadə edə bilərik.

Budur nəticə nümunəsi. Hydra ' nın uğurlu matçlara əlavə olaraq uğursuz cəhdlər haqqında məlumat yazdırdığını görürük.

Daha çox məlumat toplamaq üçün debug (-d) bayrağından da istifadə edə bilərik.

Hydra ' nın ehtiyacımızdan daha çox məlumat çıxardığını görürük. Biz nadir hallarda debug rejimindən istifadə edirik, lakin Hydra-nın xidməti zorla işə saldığı zaman həyata keçirdiyi hər bir hərəkəti izləmək imkanımız olduğunu başa düşmək xoşdur.

Nəticələrinizi Hydra-da necə saxlamaq olar

Nəticələri necə saxlayacağımızı nəzərdən keçirək. Şifrəni sındırmaq və sistemin çökməsi səbəbindən itirmək üçün saatlar sərf etməyin mənası yoxdur.

Nəticəni saxlamaq üçün -o simvolundan istifadə edə və fayl adını göstərə bilərik. Budur sintaksis.

    hydra -l <username> -p <password> <ip> <service> -o <file.txt>

Əlavə Bayraqlar və formatlar

Hydra ayrıca pen testers olaraq bizim üçün faydalı olacaq bir neçə əlavə bayraq və format təklif edir. Bunlardan bəziləri:
Xidmət spesifikasiyası

Xidməti ayrıca göstərmək əvəzinə, IP ünvanı ilə birlikdə istifadə edə bilərik. Məsələn, SSH-ni təkrarlamaq üçün aşağıdakı əmrdən istifadə edə bilərik:

    hydra -l <username> -p <password> ssh://<ip>

Hücumları necə davam etdirmək olar

Hücumun icrası zamanı Hydra sessiyası bitərsə, sıfırdan başlamaq əvəzinə -R bayrağından istifadə edərək hücumu davam etdirə bilərik.

    hydra -R

Xüsusi portlardan necə istifadə olunur

Bəzən sistem administratorları xidmət üçün standart portları dəyişdirirlər. Məsələn, FTP, standart port 3000 əvəzinə 21 portu ilə işləyə bilər. Belə hallarda -s bayrağından istifadə edərək limanları təyin edə bilərik.

    hydra -l <username> -p <password> <ip> <service> -s <port>

Birdən çox Hosta necə hücum etmək olar

Hücum etmək üçün bir neçə ev sahibi varsa nə etməli? Sadə dillə desək, -M bayrağından istifadə edə bilərik. 
Files sahəsində.txt, tək bir IP ünvanı deyil, IP ünvanlarının və ya hostların siyahısını ehtiva edəcəkdir.

    hydra -l <username> -p <password> -M <host_file.txt> <service>

Məqsədli birləşmələr

İstifadəçi adları və şifrələrin siyahısı varsa, lüğət hücumunu həyata keçirə bilərik. Ancaq ehtimal ki, hansı istifadəçi adlarında parol dəsti olduğu barədə daha çox məlumatımız varsa, Hydra üçün xüsusi bir siyahı hazırlaya bilərik.

Məsələn, aşağıda göstərildiyi kimi nöqtəli vergüllə ayrılmış istifadəçi adlarının və şifrələrinin siyahısını yarada bilərik.

    username1:password1
    username2:password2
    username3:password3

Daha sonra Hydra-ya bütün istifadəçiləri və şifrələri təkrarlamaq əvəzinə bu xüsusi birləşmələrdən istifadə etməsini söyləmək üçün '- C' bayrağından istifadə edə bilərik. Bu, kobud hücumu başa çatdırmaq üçün lazım olan vaxtı xeyli azaldır.
Budur sintaksis.

    hydra -C <combinations.txt> <ip> <service>

Hydra ilə necə işləyəcəyimizi ətraflı araşdırdıq. İndi FTP, SSH və Telnet kimi şəbəkə xidmətlərinin real auditini aparmağa hazır olmalısınız.

Ancaq təcrübəsiz bir testçi olaraq özünüzü bu hücumlardan necə qoruyacağınızı anlamaq vacibdir. Yaxşı aktyor olduğumuzu unutmayın😎.

Hydra-dan necə qorunmaq olar

Kobud güc hücumlarından qorunmağınıza kömək edəcək sadə bir həll güclü Parollar qurmaqdır. Şifrə nə qədər güclüdürsə, kobud güc metodlarını tətbiq etmək daha çətindir.

Parolların bir neçə həftədən bir dəyişdirilməsinə imkan verən parol siyasətini də tətbiq edə bilərik. Təəssüf ki, bir çox fərdi və şirkət illərdir eyni parollardan istifadə edir. Bu, onları kobud hücumlar üçün asan bir hədəf halına gətirir.

Şəbəkəyə icazəsiz girişin qarşısını almağın başqa bir yolu icazə cəhdlərini məhdudlaşdırmaqdır. Bir neçə uğursuz giriş cəhdindən sonra hesabları bloklasaq, "brutfors" hücumları işləmir. Bu, uğursuz giriş cəhdləri ilə hesabınızı bloklayan Google və Facebook kimi tətbiqlərdə yaygındır.

Nəhayət, re-captcha kimi alətlər kobud hücumların qarşısını almaq üçün əla bir yol ola bilər. Hydra kimi avtomatlaşdırma vasitələri captcha ' ları adi bir insanın etdiyi kimi tanıya bilmir.
Xülasə

Hydra, SSH və FTP kimi xidmətləri sındırmaq üçün sürətli və çevik bir şəbəkə vasitəsidir. Modul arxitekturası və paralelləşdirmə dəstəyi ilə Hydra yeni protokollar və xidmətlər daxil etmək üçün asanlıqla genişləndirilə bilər.

Hydra, şübhəsiz ki, sınaq arsenalında olmağa dəyər güclü bir vasitədir.

Ümid edirəm bu məqalə Hydra-nın necə işlədiyini anlamağa kömək etdi. Hər hansı bir sualınız varsa, şərhlərdə Mənə bildirin.

Mənimlə əlaqə saxlaya və ya gizli təhlükəsizlik bülleteninə abunə ola bilərsiniz. Məqaləni həqiqətən bəyəndinizsə, burada mənə qəhvə verə bilərsiniz.

Mənbə: [freecodecamp](https://www.freecodecamp.org/news/how-to-use-hydra-pentesting-tutorial/)