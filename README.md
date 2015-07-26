# PHPUnit Skeleton Generator

`phpunit-skelgen` เป็นโปรแกรมสำหรับช่วยสร้าง Test โดยมีหน้าที่ในการอ่านคลาสและนำมาสร้างเป็นไฟล์สำหรับใช้ทดสอบคลาสโดย UnitTest

## Installation

### PHP Archive (PHAR)

The easiest way to obtain phpunit-skelgen is to download a [PHP Archive (PHAR)](http://php.net/phar) that has all required dependencies of phpunit-skelgen bundled in a single file:

    wget https://phar.phpunit.de/phpunit-skelgen.phar
    chmod +x phpunit-skelgen.phar
    mv phpunit-skelgen.phar /usr/local/bin/phpunit-skelgen

You can also immediately use the PHAR after you have downloaded it, of course:

    wget https://phar.phpunit.de/phpunit-skelgen.phar
    php phpunit-skelgen.phar

### Composer

Simply add a dependency on `phpunit/phpunit-skeleton-generator` to your project's `composer.json` file if you use [Composer](http://getcomposer.org/) to manage the dependencies of your project. Here is a minimal example of a `composer.json` file that just defines a development-time dependency on phpunit/phpunit-skeleton-generator:

    {
        "require-dev": {
            "phpunit/phpunit-skeleton-generator": "*"
        }
    }

For a system-wide installation via Composer, you can run:

    composer global require "phpunit/phpunit-skeleton-generator=*"

Make sure you have `~/.composer/vendor/bin/` in your path.

## อัปเดท by Goragod Wiriya

พบปัญหาการใช้งานเกี่ยวกับวงเล็บถ้ามีอะไรต่อท้ายจะ error และ หากเป็นการเรียกฟังก์ชั่น

### มีการเพิ่มกฏเล็กน้อย
* สามารถกำหนดค่าเริ่มต้นที่จะใส่ลงในฟังก์ชั่น setup ได้
* สามารถกำหนด parameter ให้กับ Class ในตอนเริ่มต้นได้
* ผลลัพท์ที่เป็น string ต้องอยู่ภายใต้เครื่องหมาย "" เท่านั้น
* สามารถใส่ผลลัพท์ที่เป็นตัวเลขได้
* สามารถใส่ผลลัพท์ในรูป Array ได้
* สามารถใส่ option ได้โดยมีรูปแบบ [[....]] โดยใส่ด้านท้ายสุดของยรรทัด option จะถูกแทรกลงใน testFunction ก่อนทำการประมวลผล Test
* สามารถใช้ฟังก์ชั่นได้ โดยไม่ต้องใส่วงเล็บครอบ

### ตัวอย่างการใช้งาน setup และ setupParam
    /**
    *
    * @setupParam new Object
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
            $this->object = new myClass(new Query);
            $this->object->value1 = 'one';
            $this->object->value2 = 'true';
        }
    }

### ตัวอย่างการใช้งาน assert

    @assert (param1, param2) [==] 1
    @assert where(1)->text() [!=] "expectedResult"
    @assert (param1, param2) [==] array('one', 'two')

### ตัวอย่างเมื่อมีการใส่ option ลงใน assert

    @assert (param1, param2) [==] 1 [[$this->properties = 0]]

ผลลัพท์

    public function testCreateUrl3()
    {
        $this->object->properties = 0;
        $this->assertEquals(
            ...., .....
        );
    }