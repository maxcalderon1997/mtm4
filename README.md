#ifndef MTM4_SORTEDSET_H
#define MTM4_SORTEDSET_H

#include <functional>

template <class T>
typedef struct node{//is typedef allowed?
	T* data;
	node<T>* next;
};

template <class T, class Compare = std::less<T> >
class SortedSet {
public:
	class iterator {//do we need it to be friend?
	private:
	    node<T>* element;
	public:
    	iterator() : element(nullptr) {}
    	~iterator() = default;//default?
    	iterator(const iterator&) = default;
    	iterator& operator++(){
    		element=element->next;
    		return *this;
    	}
    	iterator operator++(int){
    		iterator old=*this;
    		element=element->next;
    		return old;
    	}
    	T operator*() const{
    		return *(element->data);
    	}
    	bool operator==(const iterator& i) const {
    		return ((element == nullptr && i.element == nullptr)
    				||(element==i.element));
    	}
    	bool operator!=(const iterator& i) const {
    		return !(*this==i);
    	}
    	void operator=(const iterator&) = default;
    };
	SortedSet() : size(0){}
	~SortedSet() = default;
	iterator begin(){
		iterator i=head;
		return i;
	}
	iterator end(){
		iterator i=nullptr;
		return i;
	}
	iterator find(const T& t){
		iterator i=begin();
		for(;i!=end()&&compare(*(i.element->data),*t);i++){}
		if(i!=end()&&i.element->data==t){
			return *(i.element->data);
		}
		return nullptr;
	}
	bool insert(const T& t){
		if (!find(t)){
			return false;
		}
		iterator i=begin();
		while(i.element->next&&compare(i.element->next->data,t)){
			i++;
		}
		iterator Titerator;
		Titerator.element->data=t;
		Titerator.element->next=i.element->next;
		i.element->next=Titerator.element;
		size++;
		return true;
	}
	bool remove(const T& t){
		if(find(t)){
			return false;
		}
		iterator i=begin();
		for(;i->element->next&&compare(*(i.element->next->data),*t);i++){}
		i.element->next=i.element->next->next;
		size--;
		return true;
	}
private:
	iterator head;
	int size; //the current size of the SortedSet
};

#endif //MTM4_SORTEDSET_H
