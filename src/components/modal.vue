<template>
		<div class="modal-mask" v-if="show" transition="modal" transition-mode="out-in">
			<div class="modal-wrapper" v-el:overlay @click.stop="modalclick" >
				<div class="modal-container">
					<div class="modal-header">
						<span class="modal-close"  @click="show=false"></span>
						<slot name="header">
						default header
						</slot>
					</div>
					<div class="modal-body">
						<slot name="body">
						default body
						</slot>
					</div>
					<div class="modal-footer">
						<slot name="footer">
							试试在input里面输入一些文字
							<input type="text" v-el:inp @keyup="modalkeyup">
							<button class="modal-default-button" @click="confirmCallback">
								OK
							</button>
							<button class="modal-default-button" v-on:click="cancelCallback">
								Cancel
							</button>
						</slot>
					</div>
				</div>
			</div>
		</div>
</template>

<script>
	module.exports = {
		props: {
			show:{
				require:true,
				type:Boolean
			},
			cancelfn: {
				type: Function,
				default: function(){}
			},
			confirmfn: {
				type: Function,
				default: function(){}
			}
		},
		methods:{
			cancelCallback:function(){
				this.cancelfn();
				this.cancelfn = function(){};
			},
			confirmCallback: function(){
				this.confirmfn();
				this.confirmfn = function(){};
			},
			keyupcallback:function(){
				console.log('duang~');
			},
			modalclick:function(e){
				// console.log(e.target);
				// console.log(this.$els.overlay);
				//因为想实现只点击背景层隐藏modal
				//处理由于冒泡，点击了里面的内容，也导致overlayer隐藏的问题
				if( e.target === this.$els.overlay ){
					//点击遮罩层，隐藏modal
					this.show = false;
				}
				console.log('modalclick');
			},
			modalkeyup:function(e){
				this.$root.modalbody = e.target.value;
			}
		}
	}
</script>

<style>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, .5);
  display: table;
  transition: opacity .3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 300px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
  transition: all .3s ease;
  font-family: Helvetica, Arial, sans-serif;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 20px 0;
}

.modal-default-button {
  float: right;
}
.modal-header .modal-close {
    display: inline-block;
    float: right;
    width: 12px;
    height: 12px;
    margin-top: 4px;
    background: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyFpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNS1jMDE0IDc5LjE1MTQ4MSwgMjAxMy8wMy8xMy0xMjowOToxNSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIChXaW5kb3dzKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo0MUIxODc5OTVCNTgxMUU1ODAzNkYzMTAwN0RDOEU0OCIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo0MUIxODc5QTVCNTgxMUU1ODAzNkYzMTAwN0RDOEU0OCI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjQxN0U4Rjc0NUI1ODExRTU4MDM2RjMxMDA3REM4RTQ4IiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjQxN0U4Rjc1NUI1ODExRTU4MDM2RjMxMDA3REM4RTQ4Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+HUcKGgAAAQFJREFUeNpi/P//PwMlgImBQkCxASzInIlTthoCqUYgPpqf492JJM4IpNKB2AuIpwPltuNyQTsQ+wJxOVBTJpJ4HBA3QeXqcLoACA4BsQ0QCwJxA9Tm91CDRYH4G8h1+AzoA+LPQAxyvhgQt0E1SQLxRyDuB+IJyBoY0aMRaCsbkIoB4rlIwiDNrVD/f8EbC0AFv4DUVyD+jSb1GV0zVgOALogEUUDMChX6DsT8QFwLlMvCawBQQQY0HMShGluAuArqBSlQFAPV1OFzQSgQSwDxDyDugBowE5o2QIErAo1KnLHQDMQcQHwMZAA0PEAumw2keIDYE4gn4Y2FoZeZAAIMALyUTmwPR6yCAAAAAElFTkSuQmCC") no-repeat center;
    cursor: pointer;
}
.modal-header .modal-close:hover {
    background: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyFpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNS1jMDE0IDc5LjE1MTQ4MSwgMjAxMy8wMy8xMy0xMjowOToxNSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIChXaW5kb3dzKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo5RThBQjBFNzVCNTgxMUU1QTIzNzgyRTJBQjExRTk1MCIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo5RThBQjBFODVCNTgxMUU1QTIzNzgyRTJBQjExRTk1MCI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjlFNjE3RDAyNUI1ODExRTVBMjM3ODJFMkFCMTFFOTUwIiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjlFNjE3RDAzNUI1ODExRTVBMjM3ODJFMkFCMTFFOTUwIi8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+thqe1wAAAPhJREFUeNpi/P//PwMlgImBQkBdA37//m0IxJuAuBxNnBGIM6BynigmgMIAhn/9+rUDiP8D8TsgzkQSjwfiV1C548h6WNBcdAiIbYBYEIgbQDYD6fdA3A7EokD8DYiP4nMBBxDnAvE3qG0fgPgZErseiPmR9aAYADWEDYiToJr+I2kuBWIedPUYscDKyvoLSH0FhR2a1Geg3BeMaMDigkggfoFkO8w7T4E4C109uuYMIH6OpLEZiAugXgCJvQbiOnwG7IUq/A5SCMTsQMwJxIVA/AkqdxpfNDYDMQcQHwPiDmh4gBLSbCDFA8SgRDQJWQPj0M9MAAEGACTic2YzEZkcAAAAAElFTkSuQmCC") no-repeat center;
}



/*
 * the following styles are auto-applied to elements with
 * v-transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

.modal-enter, .modal-leave {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}

</style>