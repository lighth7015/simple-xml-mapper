# SimpleXMLMapper
Uses reflection to map XML to PHP objects. Inspired in part by [sabre/xml](http://sabre.io/xml/valueobjects/).
## Usage Example
```xml
<?xml version="1.0" encoding="utf-8" ?>
<car>
    <manufacturer>
        <name>Volkswagen</name>
        <founded>1937-01-04 00:00:00</founded>
    </manufacturer>
    <model>Golf R</model>
    <year>2016</year>
    <msrp>39995.95</msrp>
    <colours>
        <colour>Lapiz Blue</colour>
        <colour>Oryx White</colour>
    </colours>
    <options>
        <option>
            <name>19-inch "Pretoria" Wheels</name>
        </option>
        <option>
            <name>Technology Package</name>
        </option>
    </options>
    <hybrid>false</hybrid>
</car>
```
```php
use DateTime;
use SimpleXMLElement;
use SimpleXmlMapper\XmlMapper;
use Symfony\Component\PropertyInfo\Extractor\PhpDocExtractor;

class Car
{
    /**
     * @var Manufacturer
     */
    public $manufacturer;

    /**
     * @var string
     */
    public $model;

    /**
     * @var int
     */
    public $year;

    /**
     * @var float
     */
    public $msrp;

    /**
     * @var array
     */
    public $colours = [];

    /**
     * @var Option[]
     */
    public $options = [];

    /**
     * @var bool
     */
    public $hybrid;
}

class Manufacturer
{
    /**
     * @var string
     */
    public $name;

    /**
     * @var DateTime
     */
    public $founded;
}

class Option
{
    /**
     * @var string
     */
    public $name;
}

$xml = new SimpleXMLElement(file_get_contents('test.xml'));

$mapper = new XmlMapper(new PhpDocExtractor);
$mapper->addType(DateTime::class, function ($xml) {
    return DateTime::createFromFormat('Y-m-d H:i:s', $xml);
});

$car = $mapper->map($xml, Car::class);
```
