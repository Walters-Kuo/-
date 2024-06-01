Google CR指引, 如何推進程式碼評審

Google Code Review Guidelines

Google積累了很多最佳實踐，涉及不同的開發語言、項目，這些文件，將Google工程師多年來積攢的一些最佳實踐經驗進行了總結並分享給眾開發者。學習下這裡的經驗，我們在進行項目開發、開源協同的過程中，相信也可以從中受益。

Google目前公開的最佳實踐相關文件，目前包括：

    Google’s Code Review Guidelines，Google程式碼review指引，包含以下兩個系列的內容：
        The Code Reviewer’s Guide
        The Change Author’s Guide

這了涉及到Google內部使用的一些術語，先提下：

    CL: 代表changelist，表示一個提交到VCS的修改，或者等待review的修改，也有組織稱之為change或patch；

    LGTM：代表Looks Good to ME，負責程式碼review的開發者對沒有問題的CL進行的評論，表明程式碼看上去OK；

The Code Reviewer’s Guide

從程式碼reviewer的角度出發，介紹下Google內部積累的一些good practices。
Introduction

Code Review（程式碼評審）指的是讓第三者來閱讀作者修改的程式碼，以發現程式碼中存在的問題。包括Google在內的很多公司會通能過Code Review的方式來保證程式碼和產品的質量。

前文已有提及，CR相關內容主要包括如下兩個系列：

    The Code Reviewer’s Guide
    The Change Author’s Guide

這裡先介紹下CR過程中應該做什麼，或者CR的目標是什麼。
What Do Code Reviewers Look For?

Code review應該關注如下方面：

    Design：程式設計、架構設計是否設計合理
    Functionality：程式碼功能是否符合作者預期，程式碼行為是否使用者友好
    Complexity：實現是否能簡化，程式碼可讀性是否良好，介面是否易用
    Tests：是否提供了正確、設計良好的自動化測試、單元測試
    Naming：變數名、類名、方法名等字面量的選擇是否清晰、精煉
    Comments：是否編寫了清晰的、有用的註釋
    Style：程式碼風格是否符合規範
    Documentation：修改程式碼的同時，是否同步更新了相關文件

Picking the Best Reviewers

一般，Code review之前，我們應該確定誰才是最好的、最合適的reviewer，這個reviewer應該**“有能力在比較合理的時間內對程式碼修改是否OK做出透徹、全面的判斷”**。通常reviewer應該是編寫被修改程式碼的owner，可能他是相關項目、相關原始檔、相關程式碼行的建立者或者修改者，意味著我們發起Code review時，同一個項目可能需要涉及到多個reviewer進行Code review，讓不同的、最合適的reviewer來review CL中涉及到的不同部分。

如果你心目中有一個合適的reviewer人選，但是這個人當前無法review，那麼我們至少應該“@”或者“郵件抄送”該reviewer。
In-Person Reviews

如果是結對程式設計的話，A寫的程式碼B應該有能力進行程式碼review，那麼直接找B進行review就可以了。

也可以進行現場評審（In-Person Reviews），一般是開發者介紹本次CL的主要內容、邏輯，其他reviewer對程式碼中年可能的問題、疑惑進行提問，本次CL的開發者進行解答，這種方式來發現CL中的問題也是常見的一種方式。較大型、急速上線的項目，這種方式團隊內部用的還是比較多的。
How to Do a Code Review

這裡總計了一些Code review的建議，主要包括如下一些方面：

    The Standard of Code Review
    What to Look For In a Code Review
    Navigating a CL in Review
    Speed of Code Reviews
    How to Write Code Review Comments
    Handling Pushback in Code Reviews

The Standard of Code Review

程式碼review的主要目的就是為了保證程式碼質量、產品質量，另外Google的大部分程式碼都是內部公開的，一個統一的大倉庫，通過程式碼review的方式來保證未來Google程式碼倉庫的質量，Google設計的程式碼review工具以及一系列的review規範也都是為了這個目的。

為了實現這個目標，某些方面需要做一些權衡和取捨。

首先，開發者必須能夠持續最佳化。如果開發者從來不對程式碼做最佳化，那麼最終程式碼倉庫一定會爛掉。如果一個reviewer進行程式碼review時很難快速投入，如不知道做了哪些變更，那麼程式碼reviewer也會變得很沮喪。這樣就不利於整體程式碼質量的提高。

另外，程式碼reviewer有責任維護CL中涉及到的修改的質量，要保證程式碼質量不會出現下降，時間久了也不至於爛尾。有的時候，某些團隊可能由於時間有限、趕項目，程式碼質量就可能會出現一定的下降。

還有，程式碼reviewer對自己review過的程式碼擁有owner權限，並要為程式碼後續出現的問題承擔責任。通過這種方式來進一步強化程式碼質量、一致性、可維護性。

基於上述考慮，Google制定了如下規定來作為Code review的內部標準：

    In general, reviewers should favor approving a CL once it is in a state where it definitely improves the overall code health of the system being worked on, even if the CL isn’t perfect.

    一般，reviewers對CL進行review的時候，達到approved的條件是，CL本身可能不是完美的，但是它至少應保證不會導致系統整體程式碼質量的下降。

當然，也有一些限制，例如，如果一個CL新增了一個新特性，reviewer目前不想將其新增到系統中，儘管這個CL設計良好、編碼良好，reviewer可能也會拒絕掉。

值得一提的是，沒有所謂的perfect code，只有better code。

    程式碼reviewers應該要求開發者對CL中每一行程式碼進行精雕細琢，然後再予以通過，這個要求並不過分。
    或者，程式碼reviewers需要權衡下他們建議的“精雕細琢”的必要性和重要性。程式碼reviewers應該追求程式碼的持續最佳化，不能一味地追求完美。對於提升系統可維護性、可讀性、可理解性的程式碼CL，reviewers應該盡快給出答覆，不能因為一味追求完美主義將其擱置幾天或者幾週。

Mentoring

Code review對於教授開發者一些新的東西，如程式語言、框架、軟體設計原則等是非常重要的手段。進行程式碼review的時候新增一些comments有助於幫助開發者之間分享、學習一些新東西。分享知識也是持續改進程式碼質量的重要一環。

需要注意的是，如果comments內容是純教育性的、分享性的，不是我們前面提到的強制性的必須應該做出最佳化的，那麼最好在comments內容裡面新增前綴“Nit (Not Important)”，這樣的評論表示當前CL中不一定非要做出對應的最佳化、修改，只是一個建議、分享。
Principles

    技術本身、資料至上，以及一些個人偏好
    程式碼風格的重要性，力求程式碼風格的一致，如果沒有明確的程式碼風格，就用之前作者的風格
    業內的程式碼風格、個人偏好，需要在二者之間適當平衡，reviewer也應該在程式碼風格上注意
    如果沒有明確的一些規定，reviewer可以要求CL作者遵循當前程式碼庫中的一些慣用的做法

上述各條，均以不降低系統整體程式碼質量為度量標準。
Resolving Conflicts

如果在程式碼review中出現了衝突的意見、觀點，首先，開發者、reviewer應儘可能基於之前的程式碼、現在CL的程式碼達成一個共識，如果仍然達不成共識，或者很困難，最好能進行面對面溝通，或者將當前CL升級一下，供更多的人員進行討論。可以考慮將技術Leader、項目經理拉進來一起討論下。

這種面對面的方式比單純地通過comments進行討論要高效、友好地多，如果條件不允許只能通過comments方式進行，那麼對於討論的結果，應進行適當的總結，給出一個結論，方便之後的開發者能夠瞭解之前的討論結果。

目標就是，不要因為程式碼reviewer和CL作者之間達不成一致，就長時間將CL擱置。
What to Look For In a Code Review

結合前面提到的一些Code review標準，將Code review中應該關注的點進一步細化，主要以下內容。
Design

Code review過程中最重要的事情就是看CL的整體設計是否合理，如CL中涉及到的各個部分的程式碼之間的介面、互動、銜接是否合理，是業務程式碼修改還是庫的修改，和系統整體的整合是否合理，現在這個時間點新增這個新特性是否合理等等。
Functionality

CL的功能是否符合開發者預期，開發者期望的這部分修改是否對使用者友好，這裡的使用者包括產品使用者（實際使用產品的人員）和開發者（將來可能使用這部分程式碼的人員，如CL修改的庫程式碼）。

一般，我們希望開發者發起Code review之前，能夠對CL進行充分的測試確保功能是符合預期的。但作為reviewer，還是要檢查下邊界條件的處理是否到位，比如並行中的data race問題，確保程式碼中不存在“可能”的bug。

reviewer有條件的話，也可以親自驗證下CL，比如CL是面向使用者的產品（如UI改變），單純看程式碼不能直覺地感受到做的調整，reviewer可以親自patch這部分程式碼、編譯建構、安裝之後來體驗下具體的改變。如果不是特別方便的話，也可以找相應的開發者提供一個demo演示下CL中涉及的變化。

另一個非常重要的點是，要檢查CL中是否存在某種類型的並行問題，如deadlocks、race conditions等。這些問題也不是運行一下程式碼就能發現的，往往需要reviewer來細緻地考慮下相關的操作，才能判定是否有引入該類問題。
Complexity

CL是否過於複雜，這個需要對CL中的不同層次的內容進行逐一檢查，如每行程式碼是否過於複雜，函數實現是否過於複雜，類實現是否過於複雜等。“過於複雜”意味著，不能被其他開發者快速吸收、理解。也有個笑話，開發者在呼叫、修改自己編寫的程式碼的時候容易引入引入bug。這些都說明了複雜性的問題所在。

過度設計也是Complexity中的一種，開發人員可能對當前需要解決的問題進行瞭解決，但是在此基礎上進行了過度的、不必要的延伸。reviewers需要小心判斷，識別出一個CL中是否存在過度設計的問題。reviewers應鼓勵開發人員解決當前已經存在的、明確的問題，避免開發人員做些主觀上認為未來可能需要的某些功能。
Tests

CL中應該包含必要的單元測試、整合測試等必要的測試用例，除非這個CL是為了緊急解決線上問題，這種情況下未能及時新增可以原諒。

確保CL中包含的測試用例是正確的、有意義的、有效的，不要為了寫測試用例而寫測試用例，測試用例是為了輔助驗證我們的程式碼是否符合預期的，因此幾乎任何時候，確保測試用例有效都是至關重要的，測試用例也是程式碼維護工作的一部分。
Naming

命名（變數名、函數名、類名等等）是否合理，一個好的名字應該長度適中，又能夠清晰地表達其代表什麼。
Comments

開發者是否提供了清晰、易於理解的英文註釋。

提供的註釋是否是必要的，註釋不是筆記，註釋應該用來解釋某段程式碼為什麼存在，不需要解釋這段程式碼要幹什麼，這樣的註釋才有意義。如果不提供該註釋將無法理解該程式碼，請考慮一下設計、編碼實現複雜度等是否有問題，是否可以簡化。也有一些例外情況，如正規表示式或者複雜的演算法，提供註釋註明其作用是有價值的。

當前CL之前的註釋，也需要檢查一下，可能當前CL解決了一個問題，對應的某個TODO可以刪除了。

註：這裡的註釋不同於類註釋、模組註釋、函數註釋，這些註釋需要表明其存在的目的、功能、如何使用、有什麼行為或副作用。
Style

程式碼風格問題，Google也提供了一些程式碼規範，這裡的規範涵蓋了多種程式語言的規範，請確認CL遵循此規範。

如果某些地方沒有明確的程式碼規範指引，而你有覺得開發者這種寫法不太好，你想提出點建議的話，review的時候請在意見裡面加個前綴“Nit:”，Not Import Pick，以表明這是個非強制性的建議。不要因為個人偏好問題，阻塞CL的程式碼review過程。

CL裡面不要既做程式碼邏輯修改，又做大範圍格式化的操作，這可能讓review、merge、rollback等變得複雜，如果確實有必要做這樣的調整，請提供兩個CL。第一個用來格式化，第二個在此基礎上再進行程式碼邏輯的修改。反過來也可以，但是不要一次做兩件“變動巨大”的事情。
Documentation

如果一個CL改變了使用者build、test、interact with、release code的方式，請同步檢查下文件是否也要更新，包含READMEs、g3doc pages以及其他一些生成的reference docs。如果CL刪除了或者廢棄了某些程式碼，考慮下是否對應的文件內容也有需要刪除的。

如果發現缺少某些文件內容，請聯絡開發者補齊。
Every Line

檢查review任務中你分配的每一行程式碼。對於某些自動化生成的資料檔案、程式碼檔案（如*.pb.go）、大型資料結構，你可以快速掃過去，但是如果是開發者自己寫的class、function、block of code，這種不能掃一下就過去了，需要仔細檢查。

開發者自己寫的程式碼，也有重要的、不重要的之分，某些程式碼片段需要更仔細地review，比如某些分支判斷邏輯，reviewer需要培養這方面的識別重要程式碼路徑的能力。如果目前沒有這種識別關鍵路徑的能力，也至少應該確保你看懂了這些程式碼。

如果review過程中，你發現某些程式碼實在是費解，嚴重拖慢了review進度，這種情況下不要猶豫，直接聯絡程式碼作者來解釋它是幹什麼的、能否簡化實現，然後再review。在Google僱傭了很多優秀的開發者，如果某位看不懂，那麼其他人可能也看不懂，隨著業務需求變更，將來新加入的開發者更可能看不懂。在Tencent以及其他公司，這種情況也是一樣的，reviewer應該直接聯絡程式碼作者來解釋、最佳化程式碼實現，某種意義上這也是為提升整體程式碼質量做貢獻了。

如果你理解了程式碼邏輯，但是對於某些部分你拿不準，請邀請另一位資歷更深的reviewer來review，特別是對於那些安全、並行、可用、國際化等方面複雜的問題。
Context

review過程中查看CL相關程式碼上文是有幫助的。程式碼review的時候，程式碼review工具只會顯示幾行修改的程式碼，但是前後與之相關的程式碼卻可能大範圍摺疊。有時，你不得不查看整個檔案內容來確保CL是有意義的。再比如，有時review工具只顯示了4行程式碼，但是這四行程式碼位於一個幾十上百行的方法中，如果你查看了CL的上下文，就會意識到這個函數的實現需要適當拆分成幾個小的函數。

將整個系統的程式碼作為Context來判定CL程式碼質量也是有必要的，如果這個CL中的程式碼質量低於系統整體程式碼的質量，請不要接受這樣的程式碼，這回導致整體程式碼質量的下降。
Good Things

如果CL中有比較好的做法，請告訴開發者，特別是對於你的修改建議開發者很好地完成的時候。程式碼review通常是聚焦於可能的錯誤，但是這個過程也給了我們鼓勵、學習good practices的機會。Mentoring機制，我們Tencent也有，去鼓勵新人如何做的更好，比告訴他們哪裡錯了，更有價值。
Summary

簡單總結下，Code review的時候也確保如下幾點：

    程式碼是否設計良好
    功能對於程式碼的使用者而言更友好
    UI的改變是有意義的
    並行處理是安全的
    不進行過度設計，沒有演變到那種不必要的複雜
    沒有去實現一些將來可能需要但是現在不需要的東西
    提供了有效的測試用例，包括單元測試等
    測試是有用的，測試是經過精心設計的，不要寫一堆沒用的測試
    命名選擇都是合理的，如變數名、函數名等
    註釋是有價值的、有用的，註釋解釋了why而不是what，當然也有例外
    文件中對程式碼變更也進行了合理的補充
    程式碼遵循制定好的程式碼規範

確保逐行review分配的程式碼（程式碼生成工具生成的可以快速瀏覽），當然你可以合理分配review時間對部分程式碼進行重點review，但請確保你看懂了每行程式碼。注意code context，確保提升整體程式碼質量，對於review過程中發現的good practices適當鼓勵開發者。
Navigating a CL in Review

現在我們知道了What to look for，如果review涉及到多個檔案，如何最高效地進行review呢？

    review之前，查看CL的整體表述，確認是否有意義
    首先，看CL中最有價值的部分，整體設計是否良好
    然後，再根據合理順序看CL中剩餘的部分

Step One: Take a broad view of the change

查看CL描述，先理解CL是做了什麼工作，這個CL是否有意義。如果reviewer覺得這個修改沒有意義，並決定拒絕該CL的時候，請選擇合適的措辭向開發者解釋清楚。

例如，reviewer：“哇看上去你做了很多工作，非常感謝，但是我們未來方向是要移除你這裡修改的FooWidget元件，如果你有時間，可以幫忙重構下BarWidget元件嗎？”

通能過這種方式，reviewer既向developer or contributor表明了立場、態度，也不失禮貌，保持禮貌是重要的，特別是對於開放原始碼專案，即便是你不認同貢獻者的CL，也至少應看到他嘗試進行付出。保持禮貌的review、討論、溝通有助於維護開源協同的氛圍。

如果你收到了不少CLs，但是這些都不是你想要的，可能就需要從更高角度出發來解決這個問題，比如完善下Contributing文件，告知開發者項目需要什麼，哪些方面鼓勵優先解決等等。
Step Two: Examine the main parts of the CL

找到CL涉及的最主要修改部分，優先進行review。CL中邏輯的變動可能主要集中在少數幾個檔案中，找到這些changes優先進行review，這有助於聚焦修改的主要部分，加速整體的review進度。如果CL涉及程式碼太多，很難識別哪部分是主要部分，那可以先問下開發者哪部分是主要修改，或者讓開發者把當前這次CL拆分成多個CLs，然後再發起review。

如果在review CL主要部分的時候，發現設計上明視訊記憶體在不合理的地方，reviewer應該立即傳送review comments給開發者，其他的程式碼可以不用review了，繼續review純粹是浪費時間。因為既然存在明顯的設計問題，等開發者修改之後，剩餘的要review的程式碼可能根本不存在了。

發現有明顯設計問題，立即傳送design review comments，還有兩個好處：

    開發者可能會發起一個CL之後，會立即在這個CL的基礎上繼續進行其他的修改工作。如果前面的CL存在明顯的設計問題，那麼很可能會將這裡的問題繼續帶入到後續的CL中，並繼續發起新的Code review。所以盡快傳送設計上的review意見一定程度上會避免、減少這類問題的發生，避免後續review重複解決同一類問題。
    主要的設計變更，比其他小修小補，花費的時間更多，每個開發者排需求都是有deadline的，及時傳送review意見有助於在deadline之前，讓開發者仍然由機會對設計做出調整，以保證項目進度，又能兼顧整體程式碼質量。

Step Three: Look through the rest of the CL in an appropriate sequence

一旦確定CL中主體程式碼沒有明顯的設計問題，就可以按照合理的順序review剩下的部分，比如按照邏輯處理的順序來review，reviewer可以根據邏輯處理中的過程、分支判斷來review相關的程式碼，從而確保不遺漏每一行變更。

通常review完CL的主體部分之後，review剩下的部分就相對簡單多了。有時閱讀測試用例對於review程式碼也是有幫助的，比如，通過測試用例你能清楚地知道各個函數參數的含義，如何使用該函數，當然你可以聯想到一些邊界條件，帶著這些問題去review CL主體程式碼效果也不錯。
Speed of Code Reviews
Why Should Code Reviews Be Fast?

Google對於Speed的追求，更期望的是團隊能夠更快更好地生產一個好的產品的速度，而不是個人開發者編碼的速度，當然這並不是說開發者的開發效率就不重要。團隊整體推進的速度是非常重要的，它關係到產品迭代的進度，關係到團隊的氛圍，關係到每個開發者的感受，設定是他們的日常生活。

在追求速度的過程中，Code review的速度扮演者一個比較重要的角色。如果review速度很慢，可能會發生：

    團隊整體推進的速度被嚴重拖慢。

    如果reviewer沒有對CL進行快速的響應，可能就會耽誤CL作者的其他後續工作，因為它可能要在此CL上開展其他工作。如果這裡的修改是解決一個線上bug、重要的features等，如果擱置換一個幾天、周、月，這種項目速度是不可接受的。

    開發者開始抗議或者不重視Code review。

    如果一個reviewer每隔幾天才回覆一次CL review意見，但是每次review意見都要求CL作者進行修改，對於CL作者這種體驗是非常沮喪的，開發者可能會抱怨這樣的reviewer，為什麼這麼review個程式碼這麼苛刻。但是如果reviewer能夠快速、及時地回覆review意見，這種抱怨往往會消失了。大家在意的不是CL有沒有問題，而是reviewer的怠慢、反應遲緩。所以reviewer要盡快回覆review意見。

    程式碼整體健康度會受到影響。

    如果review速度過慢，就會給開發人員、reviewer人員持續帶來更多的壓力，這意味著開發人員會提交更多不符合標準的CLs，reviewer人員需要回覆、拒絕、解釋、二次review的工作做會更多。進一步，也會降低開發者程式碼清理、重構、最佳化的積極性，導致整體程式碼質量下降。

How Fast Should Code Reviews Be?

如果當前沒有對專注度要求很高的任務的話，reviewer應該立即或者稍後進行review。

一個工作日，這個是Google內部指定的上限，在Code review發起的一個工作日內，reviewer必須進行review。

如果一個CL進行review之後，提出了修改意見，並且CL作者進行了修改，那一天之內可能要進行多輪review，儘量不要拖到第二天，跨天就不太好了。
Speed vs. Interruption

對於review速度和個人工作專注度之間需要做個權衡，如果正在進行某些對專注度要求比較高的任務，如正在寫程式碼，這個時候還是不要讓自己的工作中斷。研究表明開發者被中斷之後，再回到之前的工作，是需要比較長的時間的，為了追求review進度反而拖慢了個體開發進度，反過來也可能會拖慢項目整體進度。

所以建議在當前沒有對專注度要求很高的時間進行review，比如你寫完一段關鍵程式碼之後，或者吃完午飯、午休之後，等等，這個就要根據自己情況選擇了。
Fast Responses

我們談論Code review的速度，其實強調的是CL的review意見的回覆速度，而不是CL最終通過的速度。當然，如果review意見回覆速度夠快，CL作者修改也會更快，CL整體通過速度也會快些。

儘管CL整體通過耗時可能比較久，維持比較快的CL review意見還是很重要，CL作者不會因為review過慢而沮喪。

如果你現在太忙了，實在沒有時間對CL進行完整的review，你也可以傳送一個回覆資訊讓開發者知道你大約會在什麼時間段進行review，好讓他心理有底，他也可以繼續安排自己接下來的工作。或者你也可以邀請其他合適的review代你進行review。這裡並不是說你就要立即放下手裡的編碼工作立即去回覆，你可以選個稍微可以放鬆難點的時間去回覆，比如你剛剛寫完一段關鍵程式碼，大腦可以稍微休息幾分鐘的時候。

reviewers花費足夠的時間進行review是很重要的，時間充足，reviewer回覆“LGTM”才更有底氣。CL中每個意見的回覆還是要能快就快。
Cross-Time-Zone Reviews

如果review涉及到多地的、不同時區的開發人員，reviewer最好能在CL作者還在公司工作的時候給到回覆review意見，如果開發人員已經下班回家了，儘量在隔日開發人員上班前完成review，儘量不要中斷、delay他人的工作。
LGTM With Comments

為了加速Code review整體通過進度，有些場景下，即便開發者沒有完全完成reviewer的修改意見，reviewer可能也會給通過“LGTM” or “Approval”，這幾種情況下是允許的：

    reviewer有信心，開發者會在之後不久完成提及的修改意見
    剩餘的還未修改之處，是些微小的修改，可以後面改，或者不一定要當前開發者來修改

reviewer針對上述兩個情況，在LGTM的時候應該註明是哪種情況。LGTM With Comments對於多地、跨時區的開發者而言，是比較有價值的，因為reviewer最終給LGTM或者Approval可能要隔一天的時間呢，這個時候會比較久，如果是LGTM的同時附帶上reviewer對開發者的一點小期望，開發者後續再修改、最佳化，也是可以的，畢竟這兩種情況都是無傷大雅的事情。
Large CLs

如果開發者提交的一個CL非常大，以至於你不知道要從哪開始看起，這個時候可以要求開發者將這個大的CL拆分成幾個小的CL，CL2 build on CL1， CL3 build on CL2… 然後再進行review。這對reviewer而言是比較有幫助的，對開發者而言可能需要額外的一點工作。

如果換一個CL無法拆分成多個小的CLs，並且你眼下也沒有時間去快速地完成整體的review，可以考慮看下整體設計，回覆下對整體設計的意見，開發者可以先進行適當的最佳化。reviewer的目標，就是激勵開發者持續地對程式碼質量進行提升。
Code Review Improvements Over Time

持續地遵循上述的Code review流程，堅持下去，就會發現自己在review程式碼的時候，處理地越來越好、越來越快。開發者也能夠學習到符合程式碼質量要求的程式碼應該是什麼樣子的，後續提交的CLs也會變得越來越合理，花費的reviewer的時間也會越來越少。reviewer也會更快地進行review意見回覆，對於整體review進度delay的時間也會越來越少。

但是，但是，但是……不要為了“快”而降低Code review標準或者破壞整體程式碼的質量。
Emergencies

也存在一些非常緊急的情況，這些情況下CLs可能必須非常快地通過review過程，這種情況下review時的要求可以適當放低。在應用這裡的Emergencies指引之前，先瞭解下What is An Emergency?，區分什麼是緊急場景，什麼不是，避免濫用。
How to Write Code Review Comments
Summary

    保持友好
    解釋原因
    權衡“給出可能的方案並指出問題”、“給出可能的方案讓開發者自主決策”兩種方式
    鼓勵開發者簡化程式碼，鼓勵開發者新增註釋，而不是解釋問題有多複雜

Courtesy

review別人程式碼的時候，保持禮貌、尊重是非常重要的，至少不要讓開發者、貢獻者感覺到明顯的不適，如何做到這點呢？一個比較好的辦法就是在寫review意見的時候，只對code本身進行評論，不要對開發者本人進行評論，對於某些人稱代詞是否有必要出現，也需要斟酌下。

舉個例子：

Bad：“%#@!$ 這個場景下多執行緒並不會有太大的性能提升，為什麼你要用多執行緒？”

Good：“多執行緒並行增加了複雜性，但%#@!$ 場景下收到的性能提升並不明顯，最好用單執行緒模型代替多執行緒。”
Explain Why

上面展示的這個good example向開發者解釋了review不通過的原因，同時也解釋了為什麼多執行緒模型在這個場景 %#@!$ 下並不好，開發者就會從中學習，意識到自己的方案存在的問題，並進行最佳化。

當然不需要每次review的時候都進行大篇幅的解釋，但是適當的解釋是有必要的。
Giving Guidance

修復一個CL中存在的問題，是CL開發者的責任，而不是reviewer的責任。沒有要求reviewer要代替開發者給出一個完整的詳細設計，或者親自幫助其寫程式碼。

但是這並不意味著reviewer可以無視，不去主動幫助他人，特別是在CL開發者確實get不到reviewer的點或者束手無策的時候，reviewer可以選擇性的指出問題，並給出一個直接的指引，或者給出幾個備選方案讓開發者去對比、選擇，或者給一個簡單的設計讓開發者去進一步細化、完善，或者可以寫一個簡單的demo以供開發者參考。這也是Mentoring精神的一種發揚吧，這可以幫助開發者學習，使得Code review變得更加簡單、正向、積極。最終達成的結果也往往更好。

Code review追求的第一目標就是儘可能高品質的CL，第二目標就是提升開發者的技能，這樣後續的開發、Code review工作都會變得越來越簡單、積極，技術傳承也更加溫和、有效。
Accepting Explanations

如果reviewer有段程式碼不太懂，並請開發者進行解釋的話，最終的結果往往是開發者需要重寫這段不清晰的程式碼。偶爾，程式碼中新增註釋也是一種類似於“解釋”的回應，但是如果程式碼實現太過複雜，該重寫還是要重寫，不能用註釋解釋，套逃脫應該簡化、重構的工作。

程式碼review工具中填寫的解釋（對reviewer疑問的解釋）對將來閱讀程式碼的人的幫助微乎其微，這個很好理解，閱讀工程程式碼時，別人很少會去翻之前的review意見，而要等到翻review意見的時候，意味著這裡的程式碼可能已經需要重寫了。

通過註釋的方式來解釋程式碼的行為，往往只在很少的幾種場景下有效，比如在review一個自己不太熟悉的領域的程式碼時，如果有註釋reviewer更容易理解，但是對於熟知這個領域的人而言，這些註釋可能都是些常識，是多餘的。
Handling Pushback in Code Reviews
Handling Pushback in Code Reviews

有時候，reviewer的意見開發者會“懟”回來，可能是開發者不同意reviewer的建議，也可能是抱怨reviewer過於苛刻。
Who is Right?

當開發者不同意reviewer的建議時，reviewer應該停下來先想想他們的想法是不是對的。通常，開發者可能比reviewer更加熟悉相關程式碼，可能在某些方面他們理解的比reviewer更清楚。那麼，他們的爭論是否有意義?從程式碼健康度的角度考慮，他們的修改是否有意義？如果確實有意義，讓他們知道他們是對的，並且解決掉issue。

然而，開發者也不都是對的。有些情況下，reviewer應該更透徹地解釋為什麼他們給出的建議時正確的。一個好的解釋應該能展示出開發者角度的理解、reviewer角度的理解，以及reviewer的修改建議為什麼是有價值的。

某些情況下，reviewer可能要與開發者進行好幾輪的溝通、解釋，不管怎麼樣，保持禮貌的態度，應該讓開發者感覺到“你一直在傾聽他們在說什麼，你只是不同意他們的做法”。
Upsetting Developers

reviewers有時會覺得自己一直堅持CL程式碼的最佳化，開發者本人會不會覺得有點沮喪。有時，開發者可能會，或者剛開始時，開發者可能會，但是當他們覺得reviewer的建議有效確實幫助他提升了程式碼質量的時候，他就不會覺得沮喪了。

通常，只要在review的時候保持禮貌的溝通，開發者可能根本不會感覺到沮喪，這裡的擔心可能只是reviewer自己頭腦中的一種直覺而已。
Cleaning it Up Later

開發者針對reviewer的建議，有時會用這樣的方式，“這次先review通過吧，我後面再最佳化下”，確實有些開發者會這麼做，他們這麼做的原因，無非是不想再經過一輪新的耗時的review過程。

reviewer可能會給通過，有些開發者確實會在本次CL通過後，繼續寫一個新的CL來解決上次reviewer提到的問題，大部分是這樣的。但是也有不少開發者事後就忘了這些，並沒有提交新的CL cleanup之前遺留的問題。並不是說這些開發者就是不負責任，很可能是因為他們被其他工作填滿了，時間被擠佔了之後人就會失憶或者“選擇性”失憶，最終慢慢地就忘了cleanup。

因此，一個好的做法是，CL通過之後，reviewer應該立即通過開發者、督促開發者cleanup遺留的問題。
General Complaints About Strictness

如果之前Code review都比較鬆，後面Code review比較嚴的話，開發者剛開始肯定不適應，他們可能會抱怨多起來，這個時候，提升下Code review的速度有助於減少大家的抱怨。

對於一個團隊而言，如果是Code review從松到嚴，大家抱怨的時間可能持續的比較久，幾個月都有可能，但是最終開發者會看到嚴格執行Code review的好處，抗議聲最高的開發者可能會變成最有力的支持者，只要他能感受到嚴格之上的價值。
Resolving Conflicts

如果在review程式碼時遵循了上述Code review的建議，但是仍然遇到了一些和開發者之間難以解決的問題，請參考 前面的章節 The Standard of Code Review來瀏覽下相關的指引和原則，應該有助於解決遇到的問題或衝突。
The Change Author’s Guide

從程式碼CL開發者的角度出發，介紹下Google內部積累的一些good practices。
Writing Good CL Descriptions

CL描述用來記錄做了什麼改變、為什麼做這個改變，它會作為VCS中的提交歷史記錄下來，之後可能會有更多的開發者閱讀到這裡的CL描述。

將來，開發者也可能根據一些關鍵詞來搜尋這裡的CL描述，如果CL相關的關鍵資訊只記錄在程式碼中，在CL描述中不能予以體現的話，那麼別人定位你的CL就會異常困難。
First Line

    需要對修改進行一個精煉的總結
    描述應該是一個完整的句子
    描述後跟一個空行

CL描述的首行應該對做的修改進行一個精煉的總結、概括，然後後面跟一個空行。大部分情況下，開發者查看、檢索CL描述資訊（log資訊）是看的這些內容，所以CL描述的首行資訊是至關重要的。

通常，首行資訊應該是一個完整的句子，例如，一個好的首行表述："Delete the FizzBuzz RPC and replace it with the new system.", 下面這個則是一個不好的示例："Deleting the FizzBuzz RPC and replacing it with the new system."
Body is Informative

CL描述的剩餘部分應該詳細一點，可以包含對要解決問題的描述，以及為什麼CL中的實現是比較好的或者是最優的方案。如果該方法有什麼不足，也應該提一下。如果有一些bug編號、性能結果、設計文件之類的背景資訊，也應該包含進來，方便後來者查看。

即使CLs涉及到的改動不多，有必要的話，也需要在body部分描述下。
Bad CL Descriptions

“Fix bug”不是一個足夠充分的CL描述，修的什麼bug？為了修bug做了哪些修改？其他類似的不良CL表述包括：

    Fix build
    Add patch
    Moving code from A to B
    Add convennience functions
    kill weired URLs

上面這些是Google程式碼倉庫中撈出來的真實CL描述，作者可能認為他們的描述比較清晰了，但是實際上這樣的CL描述沒有任何意義，完全沒有起到CL描述應有的作用。
Good CL Descriptions

下面是一些比較好的CL描述示例。
Functionality change

    rpc: remove size limit on RPC server message freelist.

    Servers like FizzBuzz have very large messages and would benefit from reuse. Make the freelist larger, and add a goroutine that frees the freelist entries slowly over time, so that idle servers eventually release all freelist entries.

這個CL描述的首行資訊概述了做的修改，後面body部分又解釋了為什麼做這個修改，以及自己是怎麼做的，有什麼好處，還提供了實現相關的一些資訊。
Refactoring

    Construct a Task with a TimeKeeper to use its TimeStr and Now methods.

    Add a Now method to Task, so the borglet() getter method can be removed (which was only used by OOMCandidate to call borglet’s Now method). This replaces the methods on Borglet that delegate to a TimeKeeper.

    Allowing Tasks to supply Now is a step toward eliminating the dependency on Borglet. Eventually, collaborators that depend on getting Now from the Task should be changed to use a TimeKeeper directly, but this has been an accommodation to refactoring in small steps.

    Continuing the long-range goal of refactoring the Borglet Hierarchy.

這個CL描述的首行資訊指出了CL做了什麼，相對於以前的實現做了哪些修改。body部分介紹了實現細節、CL的context，也指出了雖然這個方案沒有那麼理想，但是也指出了未來的改進方向。同時也解釋了為什麼需要做這裡的重構。
Small CL that needs some context

    Create a Python3 build rule for status.py.

    This allows consumers who are already using this as in Python3 to depend on a rule that is next to the original status build rule instead of somewhere in their own tree. It encourages new consumers to use Python3 if they can, instead of Python2, and significantly simplifies some automated build file refactoring tools being worked on currently.

這個CL的首行資訊描述了做了什麼，body部分解釋了為什麼要做這裡的修改，並且給reviewer提供了充分的context（領域相關知識）資訊。
Review the description before submitting the CL

CLs在review過程中可能會再次修改，再次review的時候，CL描述資訊可能會與最新的修改不符，在提交CL之前review一下描述資訊是否與程式碼修改仍然一致，這個是有必要的。
Small CLs
Why Write Small CLs?

Small, simple CLs有這樣的好處：

    **Review更快。**如果提交一個大的CL，reviewer可能要專門抽30分鐘才能完成review過程，這個可能比較困難，但是如果CL拆的都比較小，reviewer每次抽個5分鐘完成一次review，多次review，回比前者更快完成。
    **Review更充分。**如果提交一個大的CL，改動東西很多情況下，reviewer需要遊走在更多程式碼中，心理上會傾向於關注更加重要的程式碼，難免某些程式碼會被降權，review可能不那麼充分。如果CL比較小，很容易就能看懂，也不需要來回翻程式碼，review的會更充分。
    **不易引入bug。**因為每次改動都比較小，CL作者不容易引入bug，reviewer也更容易發現bug。
    **如果被拒絕，可減少無謂的工作。**CL可能會被拒絕，如果一次做大量修改，被拒絕的話，CL中的所有修改動作可能都要重新來過，不管是合理的、不合理的，相當於做了更多無謂的工作。
    **更易merge。**如果提交一個大的CL，涉及修改比較多，衝突可能也會比較多，那麼解決衝突花費的時間會更多，不容易merge。小的CL在merge的時候就簡單多了。
    **設計不至於出大問題。**如果CL涉及修改比較少，那麼很容易把修改最佳化到最佳，對於reviewer的建議也更容易完成，如果CL比較大，reviewer意見、建議比較多，就很難一次完成了。
    **後續工作不至於阻塞在review上。**CL作者可能希望基於這個CL做更多工作，如果CL比較小那麼review可以很快通過，但是如果CL比較大，那麼CL作者將不得不等待更多的時間
    **merge後有問題，更易回退。**merge之後有時候也會意識到merge的程式碼有問題，如果要回退的話，小的CL更容易回退，如果CL很大，哇，涉及到的程式碼多，回退就比較痛苦、複雜。

reviewer不會因為某個CL很大，我不review了，然後給你拒絕掉，通常他們會表現的比較禮貌，告知你將這個大的CL拆分成幾個小的CLs。當你已經完成了一個CL時，再將其拆分成幾個小的CLs，其實這裡的工作量可能不少的，當然和reviewer爭辯為什麼要求拆分成多個CLs花的時間可能也會很長。最省力的做法就是，一開始就堅持寫小的CL，多次CLs。
What is Small?

一般，CL一般建議的大小：

    CL包含最少內容，一次只做一件事情，比如新增feature，CL只包含feature的一個部分，而不是所有的部分。也可以與reviewer進行溝通，確定合適的CL大小，不同的reviewer習慣也不一樣，對reviewer而言，CL太大太小都是負擔。我們的建議是CL一次只做一件事情。
    reviewer需要看到的任何東西都已經包含在了CL中，這些資訊可能位於CL程式碼中、描述中、已經存在的程式碼中、reviewer之前review過的CL中。
    當把這個CL合入之後，系統仍然能夠工作的很好。
    CL也不小到隱晦難懂的程度。比如新增了一個API，應該同時包含API的使用示例，這樣reviewer能夠更好地理解API的使用方式，這樣也可以避免引入沒有用的API。

也沒有硬性的規定指明CL多大才算大，一般100行程式碼是一個比較合理的CL尺寸，1000行有點太大了，主要還是要看reviewer的態度。CL中涉及到的檔案的數量也是CL大小的一個度量維度，如果CL中包含了一個檔案，檔案修改了200行程式碼，這個感覺還是OK的，但是如果同樣是修改了200行程式碼但是散落在50個檔案中，那CL有點太大了。

體會一下，我們寫程式碼的時候是有瞭解對應的背景的，但是reviewer可能瞭解比較少或者完全不知道。一個可接受的CL大小，對reviewer而言是很重要的。如果你不確定reviewer期待的CL大小是怎樣的，那就儘量寫一個比自己預期中的合理大小更小點的，很少有reviewer會嫌棄CL太小。
When are Large CLs OK?

下面這些場景，如果CL比較大也還OK，不算壞：

    刪除整個檔案的內容，或者刪除大段內容，可以看做是一行修改，因為它不會花費reviewer太多時間review。
    有時一個大的CL是有自動化refactor工具生成的，而我們信任這些工具，reviewer的工作就是檢查並確認這裡的修改沒問題。這樣的CL很大，但是對reviewer而言仍然不會花太多時間。

Splitting by Files

另一種拆分大的CL的方式是，將修改的檔案進行適當分組，並將不同分組的檔案做成CL並提交不同的reviewer進行review。

例如：你傳送了一個CL修改，同時包含了對protocol buffer檔案的修改，另一個CL是業務程式碼修改但是使用了這裡修改後的protocol buffer檔案。首先我們要先提交proto檔案對應的CL，然後再提交引用它的業務程式碼CL。雖然提交的時候有先後，但是review過程可以是同時進行的。這麼做的同時，最好也知會下reviewer另一個CL中的存在以及與當前CL的關係。
Separate Out Refactorings

一般將CL中的重構之類的工作和其中包含的feature開發、bug修改區分開是比較好的做法。例如，將CL中包含的移動、重新命名類名之類的操作單獨放在一個CL中，這樣reviewer能夠更容易理解每個CL中的修改。

不過，一個小的變數名修改的也可以包含在另一個feature change或者bugfix相關的CL中。如果一個CL中包含了一些重構之類的修改，讓開發者、reviewer判斷這部分重構相關的修改是否對當前CL來說太大了，太大了會讓review變得更加困難，開發者應該據此作出一些調整。
Keep related test code in the same CL

避免將相關的測試程式碼單獨放在另一個CL中。測試程式碼驗證程式碼修改的正確性，所以和程式碼修改相關的測試程式碼，應該放置在一個相同的CL中，儘管測試程式碼增加了修改程式碼的行數。

不相關的測試程式碼修改，可以放到另一個CL中，類似於 Separate Out Refactorings， 包含：

    驗證已經存在的、提交過的程式碼，當前只是新增、完善測試程式碼；
    重構測試程式碼（如引入了新的helper函數）
    引入了更大的測試框架（如整合測試）

Don’t Break the Build

如果多個CLs互相依賴，需要找一個辦法來確保這幾個CL每提交一個，系統整體仍然能夠正常工作，不能因為提交了一個CL就導致系統建構失敗或者工作不正常。比如，可以考慮將幾個依賴的小的CL修改合併。
Can’t Make it Small Enough

有時會遇到這種情況，看上去CL會無可避免地變得很大，其實，這種情況幾乎是不存在的。只要開發者嘗試練習去寫小的CLs多次code review，習慣了之後開發者總能找到合適的辦法來將一個大的修改分解成幾個小的修改。

在開發者動手進行一個很大的CL之前，開發者應考慮下是否先來一次只包含重構（只修改設計，不變動功能）的CL然後在此基礎上再做修改，這樣是不是會更好一點。如果一個CL中既包含重構程式碼，又包含邏輯變動程式碼，這個修改就很重。多數情況下，先來一次只包含重構的CL會為後續的修改掃清障礙，後續的修改會更clean。

如果因為某些客觀原因，無法做到上述這些，那就先提前向reviewer說明情況，讓reviewer做好心理準備，準備好review一個大的CL，review可能花費比較長的時間，請務必小心引入bug、務必新增好測試用例並通過測試，reviewer在這麼大的CL中review的效果可能會大打折扣。
How to Handle Reviewer Comments

當開發者針對CL發起Code review時，reviewer一般會通過comments寫一些意見、建議，那開發者該如何處理reviewer comments呢？
Don’t Take it Personally

Code review的初衷是為了維護程式碼的質量、產品的質量，當一個reviewer對開發者程式碼寫了些意見時，開發者應該將其看作是來自reviewer的幫助，不要將其看做是對開發者個人、個人能力的人身攻擊。

有時，reviewers會變得很沮喪，並且可能將沮喪、不滿情緒在評論中體現出來。一個好的reviewer是不會這麼做的，但是作為一個好的開發者，應該做好面對這種情況的準備。開發者可以反問下自己，當reviewer的評論到底是在描述一個什麼程式碼問題，然後嘗試去修改就可以了。

**對於別人的review意見，永遠不要用憤怒去回應！**這違反了Code review或者開源協同的精神，並且這樣的負面資訊可能會永遠留存在review歷史中。如果你心裡面很憤怒，並且想憤怒地做出回應，請嘗試離開你的電腦、鍵盤，或者先幹點別的，直到你內心平靜下來再禮貌地做出回應。

一般，如果reviewer回覆的評審意見不是建設性的，或者沒有禮貌，可以試著向reviewer私下裡訴說下你的看法。如果能面對面交流最好，不能就嘗試發一封私人郵件，向reviewer說下你不喜歡他的這種review方式，你期望他能做出好的改變。如果對方還是拒絕做出改變，那就不要浪費時間了，升級一下知會你的項目經理。
Fix the Code

如果reviewer說他看不懂你的程式碼中的某個部分，首先你應該嘗試簡化這裡的程式碼實現。如果程式碼已經無法再精簡實現，那就在程式碼中新增合適的註釋來解釋。如果新增額外的註釋沒有什麼太大意義，只有這種情況下，可以嘗試通過code review工具新增一些comments來解釋。

如果某個reviewer看不懂這裡的程式碼，那麼將來可能其他開發者閱讀程式碼的時候，也是看不懂的。但是通過code review工具新增comments進行解釋，對將來的開發者是沒有幫助的，但是精簡程式碼實現、程式碼新增註釋是有幫助的。
