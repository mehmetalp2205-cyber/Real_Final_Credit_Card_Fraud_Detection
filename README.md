Bu raporda, veri bilimindeki en klasik problemlerden biri olan aşırı dengesiz veri setleri (Imbalanced Data) üzerinde çalıştığım makine öğrenmesi projemi bulabilirsiniz.

Projenin Amacı ve Karşılaşılan Zorluk Kullandığım veri setindeki işlemlerin %99.8'i normal, sadece %0.17'si dolandırıcılık (Fraud) işlemiydi. Bu kadar dengesiz bir veride standart bir model eğitirseniz, model kolaya kaçıp her işleme "Normal" diyerek %99.8 doğruluk (accuracy) elde edebilir ama asıl amacı olan dolandırıcıları yakalayamaz. Amacım bu yanılgıyı kırmaktı.

Çözüm Yaklaşımım: "Ceza Sistemi" (Class Weights) Bu projede azınlık sınıfını çoğaltmak için sahte veri üretmek (örneğin SMOTE) yerine, verinin tamamen orijinal yapısını korumayı seçtim.

Bunun yerine modellere class_weight='balanced' parametresi ile bir ceza sistemi uyguladım. Modele temelde şu kuralı öğrettim: "Masum bir işleme yanlış alarm vermenin cezası küçüktür, ama gerçek bir dolandırıcıyı gözden kaçırmanın cezası çok ağırdır!"

Denediğim Modeller Büyük finalde 5 farklı modeli kıyasladım:

Lojistik Regresyon (Ceza sistemli)

Random Forest (Ceza sistemli)

Karar Ağacı / Decision Tree (Ceza sistemli)

KNN (Ceza mantığını desteklemiyor)

Naive Bayes (Ceza mantığını desteklemiyor)

Çıkan Sonuçlar Modelleri yanıltıcı olan Accuracy yerine ROC-AUC skoru ve Hata Matrisi (Confusion Matrix) ile test ettim:

Kazanan (Lojistik Regresyon): Ceza sistemini en iyi anlayan model oldu. 95 gerçek dolandırıcının 83'ünü başarıyla yakalayarak en güvenilir sonuçları verdi (ROC-AUC: 0.9657).

Müşteri Dostu (Random Forest): 56 bin normal işlemde sadece 1 yanlış alarm verdi ancak fazla garantici olduğu için 26 dolandırıcıyı kaçırdı.

KNN ve Naive Bayes ise ceza mantığını kavrayamadıkları için dolandırıcıları yakalamada alt sıralarda kaldı.
