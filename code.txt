
// array of objects 

// let  myarray = [{"name":"ahmad","job":function(){
//     console.log("teaching")
// }},{},{}]

// incase the express is not installed or give an error 

// Set-ExecutionPolicy RemoteSigned

const { error } = require("console");
const express = require("express")

const app = express(); 

app.use(express.json())

class Book {
    constructor(id, name, title) {

        this.id = id
        this.name = name
        this.title = title


    }

    ChangeTranslation(language) {

        // template literal 

        this.title = `${this.title} - (${language})`

    }

    static validate(book) {
        if (!(book instanceof Book)) return 'book must be instance of the book class'
        if (!book.id || typeof book.id !== "number") return 'Invalid or missing ID'
        if (!book.name || typeof book.name !== "string") return 'invalid or missing Name'
        if (!book.title || typeof book.title !== "string") return 'invalid or missing title'

        return null ; 

    }



} 

let books = []


// add new book logic 

app.post("/books",(req,res)=>{

    const {id,name,title}= req.body

    if(books.some((book)=>book.id===id)){
        return res.status(400).json({error:"this book already exist"})
    }


    const newBook = new Book(id,name,title)

    const error = Book.validate(newBook)

    if(error) return res.status(400).json({error})

        books.push(newBook)
        res.status(201).json({message:"book has been added",book : newBook})

})




const port = 3001 ; 
app.listen(port, ()=>{
    console.log(`library system is started on http://localhost${port}`)
})
