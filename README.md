# eye_tracker_3
3. göz takip fonksiyonu denemesi
4. Bu kodda Open CV ile beraber Mediapipe kütüphanesi de eklenerek daha hassa bir takip yapmaya çalışılıyor.
5. "import mediapipe as mp"
6. Şeklinde ekliyoruz.
7. Ardından "mp_face_mesh = mp.solutions.face_mesh" ifadesiyle Mediapipe'ın Face Mesh modülünü çağırıyotuz. Bu modül yapay zeka öğrenmesi ile yüz hatlarını tanımlamak için eğitilmiş bir modüldür.
8. Mediapipe tespit ettiği her yüz için 468 farklı nokta yani (landmark) elde eder. Böylece tespit ettiği yüzün belirli bölgelerine bu noktaları yerleştirir. Daha sonra biz bu noktaları çağırarak istediğimiz bölgeleri kulanacağız.
9. ![109521608-72aeed00-7ae8-11eb-9539-e07c406cc65b](https://github.com/user-attachments/assets/4b8befe3-5bcf-4257-bf42-60fe02ab749f)

10. "face_mesh = mp_face_mesh.FaceMesh(refine_landmarks=True)" FaceMesh fonksiyonunu çağırarak landmarkları oluşturuyor ve face_mesh değişkeninde tutuyoruz. "(refine_landmarks = True)" parametresi FaceMesh fonksiyonu için etkinleştirilerek normalde 468 nokta içerisinde hesaplanmayan göz bebekleri gibi küçük ayrıntılarıda hesaplıyor.
11. "cap = cv2.VideoCapture(0)" Open CV ile webcam e erişiyoruz ve kameranın anlık olarak gördüğü frame i yakalıyoruz.
"while cap.isOpened():
    ret, frame = cap.read()" isOpened fonksiyonu ile frame okumanın başarılı olup olmadığını kontrol ediyoruz. Frame okunuyorsa döngüye girilerek alttaki kodlar çalıştırılıyor.
    "if not ret:
        break" eğer frame okuma başarısız olmuşsa döngü kırılarak çıkılıyor.
    "rgb_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)" Open CV kütüphanesi görselleri BGR(Blue-Green-Red) formatıyla işler ancak Mediapie RGB(Red-Green-Blue) formatını kullanır. o yüzden Open CV ile yakaladığımız frameleri Mediapipe da işleyebilmek adına cvtColor fonksiyonuyla düzenliyoruz.
    "results = face_mesh.process(rgb_frame)" şimdide face_mesh değişkenine atadığımız modül ile yakalanan her framede noktalar oluşturularak "results" da tutuluyor.
    "if results.multi_face_landmarks:"  burada if yapısıyla result un içerisinde bir yada birden fazla herhangi bir yüzyakalanıp yakalanılmadığını kontrol ediyoruz. Yakalanmış bir yüz varsa kod devam ediyor.
    "for face_landmarks in results.multi_face_landmarks:" result un içinde bulunan landmark lar için işlenecek kodu bu koşulun içine yazıyoruz.
    " for landmark in face_landmarks.landmark[468:478]:" 468-478 arasındaki landmark lar için kullanılacak kodu da bu koşulun içine yazacağız. Dha önce yukarıda da dediğimiz gibi "refine_landmars = True" parametresi ile normalde 468 nokta oluştran FaceMesh yapısı fazladan ayrıntıyla göz bebeklerini de tespit ediyor.
    Ardından diğer yaygın Open CV fonksiyonlarıyla bulunan bu noktalara içi dolu dağreler çizerek her frame i tek tek ekrande gösteriyoruz.
    "waitKey()" fonksiyonuyla kodun durdurulması için "q" tuşuna basılması gerekn bir kontrol sistemi oluşturup yakalana frameleri serbest bırakarak, bütün pencereli kapatıp kodun çalışmasını bitiriyoruz.
