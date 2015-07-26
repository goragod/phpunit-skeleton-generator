# PHPUnit Skeleton Generator

`phpunit-skelgen` เป็นโปรแกรมสำหรับช่วยสร้าง Test โดยมีหน้าที่ในการอ่านคลาสและนำมาสร้างเป็นไฟล์สำหรับใช้ทดสอบโดย PHPUnit

## การติดตั้งจาก GitHub

* ให้ทำการ clone สคริปต์ หรือ ดาวน์โหลด script จาก GitHub ไปไว้ในเครื่องได้เลยครับ
* เปิด `terminal` ย้ายโฟลเดอร์ไปยังโฟลเดอร์ที่ติดตั้งโปรแกรมไว้

    cd /path/to/phpunit_skeleton_generator/

* ดาวน์โหลด composer และติดตั้งโปรแกรมที่จำเป็น โดย composer

    wget http://getcomposer.org/composer.phar
    php composer.phar install

* รอให้ composer ทำการติดตั้งสคริปต์ที่จำเป็นจนแล้วเสร็จ หลังจากนั้นก็ใช้งานได้เลย
* การใช้งาน เนื่องจากผมใช้งานร่วมกับ NetBeans ผมตั้งค่าตามนี้ https://netbeans.org/kb/docs/php/phpunit.html
* โดยในช่อง `Skeleton Generator scripts` ผมเลือกไปที่ไฟล์ `phpunit-skelgen` ที่อยู่ภายใต้โฟลเดอร์ที่ติดตั้งไว้

## อัปเดท by `Goragod Wiriya`

พบปัญหาการใช้งานเกี่ยวกับวงเล็บถ้ามีอะไรต่อท้ายจะ error และ หากเป็นการเรียกฟังก์ชั่น และเพิ่มเติมความสามารถให้กับ Generator
* สามารถกำหนดค่าเริ่มต้นที่จะใส่ลงในฟังก์ชั่น setup ได้ `@setup`
* สามารถกำหนด parameter ให้กับ Class ในตอนเริ่มต้นได้ `@setupParam`
* ผลลัพท์ที่เป็น string ต้องอยู่ภายใต้เครื่องหมาย `""` เท่านั้น
* สามารถใส่ผลลัพท์ที่เป็นตัวเลขได้
* สามารถใส่ผลลัพท์ในรูป Array ได้
* สามารถใส่ option ได้โดยมีรูปแบบ `[[....]]` โดยใส่ไว้ด้านท้ายสุดของบรรทัด option จะถูกแทรกลงใน testFunction ก่อนทำการประมวลผล Test
* สามารถใช้ฟังก์ชั่นได้ โดยไม่ต้องใส่วงเล็บครอบ
* operator สำหรับการเปรียบเทียบต้องอยู่ภายใต้ `[...]` เท่านั้น เช่น `[==]`, `[>]`

### ตัวอย่างการใช้งาน @setup และ @setupParam

@setup และ @setupParam จะอยู่ในส่วน Comment ของคลาสเท่านั้น

    /**
    *
    * @setupParam new className
    * @setup $this->value1 = 'one';
    * @setup $this->value2 = 'true';
    */
    myClass {

    }

ผลลัพท์

    class myClassTest extends \PHPUnit_Framework_TestCase
    {

        protected $object;

        protected function setUp()
        {
            $this->object = new myClass(new className);
            $this->object->value1 = 'one';
            $this->object->value2 = 'true';
        }
    }

### ตัวอย่างการใช้งาน @assert

    @assert (param1, param2) [==] 1
    @assert where(1)->text() [!=] "expectedResult"
    @assert (param1, param2) [==] array('one', 'two')

### ตัวอย่างเมื่อมีการใส่ option ลงใน assert

    @assert (param1, param2) [==] expectedResult [[$this->properties = 0]]

ผลลัพท์

    public function testMyFunction()
    {
        $this->object->properties = 0;

        $this->assertEquals(
            expectedResult, $this->object->myFunction(param1, param2)
        );
    }