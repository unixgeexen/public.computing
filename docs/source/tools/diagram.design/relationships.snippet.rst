.. kroki::
   :caption: UML Relationships
   :type: plantuml

   @startuml
   Class01 "1" *-- "many" Class02 : composition (contains)
   Class03 o-- Class04 : aggregation
   Class05 --> "1" Class06 : association
   Class07 <|-- Class08 : inheritance
   Class09 ..> "1" Class10 : dependency
   Class11 <|.. Class12 : realisation/implementation
   @enduml
