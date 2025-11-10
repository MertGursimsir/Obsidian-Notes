**DATA PARTITIONING**
	==Vertical Partition:==
		Separate table by columns
		Allows frequently accessed data to be stored separately
		Large objects like image or long text can be segregated
	==Horizontal Partition:==
		Divides tables by rows
		Typically using a partition key to determine which rows belong which partition
		For example store $0-$50, $50-$100, in another partitions

Partitioning can reduce I/O for mixed workloads by aligning data placement with access patterns. Performance gains come from more efficient memory utilization and reducing data scanning during query execution.


**DATABASE SHARDING**
Like horizontal partitioning but in multiple independent databases.
Sharding, sistem büyüyünce “tek motorla olmaz bu iş” deyip çok motorlu sisteme geçmek gibi.
Almanya’daki kullanıcıya Hollanda’daki sunucudan cevap vermek yavaş olur.
👉 O yüzden Almanya için ayrı bir DB orada kurarsın, hız kazanırsın.

==Hashbase Sharding:==
![[Pasted image 20250611140225.png]]
Distributes data evenly but making range queries inefficient.

==Rangebase Sharding:==
![[Pasted image 20250611140740.png]]
Optimized for range queries.

==Directorybase Sharding:==
![[Pasted image 20250611140921.png]]
Map keys to shards.
More flexibility, but more complexity.
![[Pasted image 20250611141536.png]]





![[Pasted image 20250611141418.png]]



**INDEX**
Sen index dediğinde, sistem şunu yapıyor:

"Tamam patron, ben bu kolonu özel bir veri yapısıyla tutayım ki hızlıca arayayım."

📁 Genelde bu yapı:
✅ B-tree (Balanced Tree) ← en yaygını
✅ Hash Index
✅ GiST, GIN (PostgreSQL özel)


Index yoksa baştan sona tek tek okunur.

Her INSERT, UPDATE, DELETE işlemi sırasında:

Ana tabloya veri yazılır

Index de güncellenir!

🧱 Yani index = daha hızlı okuma ama biraz daha yavaş yazma

Index demek:
🔁 Veriyi ikinci kez sakla
➕ Arama yapısı kur
🗃 Dosya olarak ayrı tut
➕ Güncellemelerde senkron ol