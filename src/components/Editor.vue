<template>
  <div>
    <div id="editor" v-on:keyup="keyUpHandler"></div>
  	<div id="content"></div>
  </div>
</template>

<script>

import {EditorState} from "prosemirror-state"
import {EditorView} from "prosemirror-view"
import {Schema, DOMParser} from "prosemirror-model"
import {schema} from "prosemirror-schema-basic"
import {addListNodes} from "prosemirror-schema-list"
import {exampleSetup} from "prosemirror-example-setup"
import {Plugin} from "prosemirror-state"
import {toggleMark, setBlockType, wrapIn} from "prosemirror-commands"


	export default {
	  name: 'Editor',
	  props: ['msg'],
	  data:function(){
	      return{
	          view:null
	      }
	  },
	  methods:{
	  	keyUpHandler(){
	  		console.log(JSON.stringify(this.view.state.tr.doc))
	  	}
	  },
	  mounted(){
		const mySchema = new Schema({
		  nodes: addListNodes(schema.spec.nodes, "paragraph block*", "block"),
		  marks: schema.spec.marks
		})



	class FloatingToolMenu {
	  constructor(view,items) {
	  	
	  	this.items = items
	  	this.view=view
	    this.floatingToolBar = document.createElement("div")
	    this.floatingToolBar.className = "toolbar"
	    this.position=1
	    this.totalElement=this.view.dom.children
	    this.lastEle=false
	    
	    this.items.forEach(({dom}) => this.floatingToolBar.appendChild(dom))
	    

	    view.dom.parentNode.appendChild(this.floatingToolBar)

	  

	    this.update(view, null)

	    this.floatingToolBar.addEventListener("mousedown", e => {
	         e.preventDefault()
	      view.focus()
	      items.forEach(({command, dom}) => {
	        if (dom.contains(e.target))

	          command(view.state, view.dispatch, view)
		      this.floatingToolBar.classList.add('inactive')
			  this.floatingToolBar.classList.remove('active')
	      })
	    })
		

	    this.floatingToolBar.querySelectorAll('span').forEach(item=>item.addEventListener('mousedown',e=>{
	
	  		 
	    	this.view.dom.lastChild.classList.add('menuItemClicked')
	 
	    	
	    	e.target.classList.toggle('active')
	    }))
	    document.querySelector('#editor').addEventListener('keyup',e=>{


	    	 if(e.keyCode==8){
	    	 	if(this.lastEle){
	    			console.log('wronggg')
	    			this.lastEle=false
		    	    this.floatingToolBar.classList.remove('active')
					this.floatingToolBar.classList.add('inactive')	  
	    			this.position--;

	    			if(this.position<1){
	    				this.position=1;
	    				this.lastEle=false;
	    				this.floatingToolBar.classList.add('active')
					    this.floatingToolBar.classList.remove('inactive')	
	    			}

	    			return;
			    }	

	    	 	if(!view.dom.children[this.position-1]){
	    	 		this.lastEle=false
	    		this.floatingToolBar.classList.remove('active')
				this.floatingToolBar.classList.add('inactive')	  
	    			this.position--;

	    			return;
	    	 	}

	    
    		 if(view.dom.children[this.position-1].textContent =='') {
    		 	    this.lastEle=true;
	    			console.log('asche')
		    		this.floatingToolBar.classList.add('active')
					this.floatingToolBar.classList.remove('inactive')
		    		const top=this.view.dom.querySelector(`*:nth-child(${this.position})`).getBoundingClientRect().top;
					const left=this.view.dom.querySelector(`*:nth-child(${this.position})`).getBoundingClientRect().left;

					this.floatingToolBar.style.left = left-10 + "px"
				    this.floatingToolBar.style.top = top-10 + "px" 	
	    		}



	    		
	    	
	    		
	    		//console.log(this.view.dom.querySelector(`*:nth-child(${this.position})`).getBoundingClientRect().top)
	    		//console.log(this.view.dom.querySelector(`*:nth-child(${this.position})`).textContent)
	    		
	    	}
	    	if(e.keyCode==13){
	    		this.position++

	    		//console.log(this.view.dom.querySelector(`*:nth-child(${this.position})`).getBoundingClientRect().top)
	    		//console.log(this.view.dom.querySelector(`*:nth-child(${this.position})`).textContent)
	    		
	    	}
	    	 if(e.keyCode==40){
	    	 	const totalElement=this.view.dom.children;
	    	 	
	    		this.position++
	    	
	    		if(this.position>totalElement.length){
	    			this.position=totalElement.length;
	    			
	    		}
	    		//console.log(this.view.dom.querySelector(`*:nth-child(${this.position})`).textContent)
	    		
	    	}

	       if(e.keyCode==38){

	    		this.position--
	    		if(this.position<1){
	    			this.position=1;
	    		}
	    		//console.log(this.view.dom.querySelector(`*:nth-child(${this.position})`).textContent)
	    		
	    	}
	    	if(this.view.dom.querySelector(`*:nth-child(${this.position})`).textContent==''){
	    		console.log('null')
	    
	    		this.floatingToolBar.classList.add('active')
				this.floatingToolBar.classList.remove('inactive')
	    		const top=this.view.dom.querySelector(`*:nth-child(${this.position})`).getBoundingClientRect().top;
				const left=this.view.dom.querySelector(`*:nth-child(${this.position})`).getBoundingClientRect().left;

				this.floatingToolBar.style.left = left-10 + "px"
			    this.floatingToolBar.style.top = top-10 + "px"
	    	}else{
	    		console.log('text')
	    		this.lastEle=false;
    		    this.floatingToolBar.classList.remove('active')
				this.floatingToolBar.classList.add('inactive')	    		
	    	}
	    	console.log('------',this.lastEle)
		    const totalElement=Array.from(this.totalElement);
		 
		    totalElement.forEach((el,index)=>{
		    	el.addEventListener('click',e=>{
	    						    		const {top,left}=e.target.getBoundingClientRect();
		    		
			    	if(e.target.textContent==''){

				    		this.floatingToolBar.classList.add('active')
							this.floatingToolBar.classList.remove('inactive')	



							this.floatingToolBar.style.left = left-10 + "px"
						    this.floatingToolBar.style.top = top-10 + "px"
			    	}else{
		    		    this.floatingToolBar.classList.remove('active')
						this.floatingToolBar.classList.add('inactive')			
			    	}
		    	
					this.position=index+1;

		    	})
		    })

		   


	    })
	  }
	  update(view){
		console.log(view)  
	  }

	  destroy() { 

	  	this.floatingToolBar.remove()
	  
	  }
	}




	// Helper function to create menu icons
	function icon(text, name) {
	  let span = document.createElement("span")
	  span.className = "menuicon " + name
	  span.title = name
	  span.textContent = text
	  return span
	}

	// Create an icon for a heading at the given level
	function heading(level) {
	  return {
	    command: setBlockType(mySchema.nodes.heading, {level}),
	    dom: icon("H" + level, "heading")
	  }
	}
	let allCommands=[
	  {command: toggleMark(mySchema.marks.strong), dom: icon("B", "strong")},
	  {command: toggleMark(mySchema.marks.em), dom: icon("i", "em")},
	  {command: setBlockType(mySchema.nodes.paragraph), dom: icon("p", "paragraph")},
	  heading(1), heading(2), heading(3),
	  {command: wrapIn(mySchema.nodes.blockquote), dom: icon(">", "blockquote")}
	]


	let floatingMenuPlugin = new Plugin({
	  view(editorView) { return new FloatingToolMenu(editorView,allCommands) },

	})

	let clickPlugin = new Plugin({
	  props: {
	  handleClickOn(view, pos, node, nodePos, event, direct){
	      console.log(view, pos, node, nodePos, event, direct)
	     const {top,left}=view.coordsAtPos(pos);
console.log(top,left)
	      console.log("A button was pressed!")     
	    }
	  }
	})


	let view  = new EditorView(document.querySelector("#editor"), {
	  state: EditorState.create({
	    doc: DOMParser.fromSchema(mySchema).parse(document.querySelector("#content")),
	    plugins: exampleSetup({schema: mySchema}).concat(floatingMenuPlugin).concat(clickPlugin)
	  })
	})

	this.view=view;

	let transaction=this.view.state.tr;
  	 	
  	transaction.insertText(JSON.parse(this.msg).greeting)
    let newState=this.view.state.apply(transaction)
  	this.view.updateState(newState)

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
