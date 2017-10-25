## Amazon - distance of two nodes in BST ##

OA原题描述参考：
注意：具体抽到的题目可能会有细微变化。
给一个无序integer array，要求用insert的方法建一个BST，然后给出其
中两个值在树上的距离，若是有不在树里的，返回-1
解题思路参考：
千万注意这是一道大题！坑：首先给的integer array可能是有序的，就不需要排序
了，否则可能会有case不过。再次是BST，不要按照BinaryTree搞，会超时导致
case不过。解题思路，先建BST，再做LCA，然后算两点和root的距离（level）。
geekforgeek上的解释：http://www.geeksforgeeks.org/find-distance-two-givennodes/

	// ConsoleApplication1.cpp : Defines the entry point for the console application.
	//
	
	#include "stdafx.h"
	#include <string>
	#include <algorithm>
	#include <vector>
	#include <stack>
	#include <unordered_map>
	using namespace std;
	
	class TreeNode {
	public:
		int val;
		TreeNode *left, *right;
		TreeNode(int x) : val(x), left(NULL), right(NULL) {}
	};
	
	class Solution {
	public:
	
		TreeNode* buildBST(int* a, int n) {
			if (n < 1) {
				return NULL;
			}
			TreeNode* root = new TreeNode(a[0]);
			for (int i = 1; i < n; i++) {
				TreeNode* node = new TreeNode(a[i]);
				insertNode(root, node);
			}
			return root;
		}
	
		TreeNode* insertNode(TreeNode* root, TreeNode* node) {
			if (root == NULL) {
				return node;
			}
			if (node -> val < root -> val) {
				root -> left = insertNode(root->left, node);
			}
			else {
				root -> right = insertNode(root -> right, node);		
			}
			return root;
		}
	
		//TreeNode* insertNode(TreeNode* root, TreeNode* node) {
		//	if (root == NULL) {
		//		return node;
		//	}
		//	TreeNode* cur = root;
		//	while (cur != node) {
		//		if (node -> val < cur -> val) {
		//			if (cur -> left == NULL) {
		//				cur->left = node;
		//			}
		//			cur = cur->left;
		//		}
		//		else {
		//			if (cur -> right == NULL) {
		//				cur->right = node;
		//			}
		//			cur = cur->right;
		//		}		
		//	}
		//	return root;
		//}
	
		TreeNode* findNode(TreeNode* root, int x) {
			if (root == NULL) {
				return NULL;
			}
			if (x < root -> val) {
				return findNode(root -> left, x);
			} 
			if (x > root -> val) {
				return findNode(root -> right, x);
			}
			return root;
		}
	
		TreeNode* findLCA(TreeNode* root, TreeNode* A, TreeNode* B) {
			if (root == NULL || root == A || root == B) {
				return root;
			}
			TreeNode* left = findLCA(root->left, A, B);
			TreeNode* right = findLCA(root->right, A, B);
			if (left != NULL && right != NULL) {
				return root;
			}
			if (left != NULL) {
				return left;
			}
			if (right != NULL) {
				return right;
			}
			return NULL;
		}
	
		int calDis(TreeNode* root, TreeNode* A) {
			int dis = 0;
			if (root == NULL) {
				return dis;		
			}
			while (root != A) {
				if (A -> val < root -> val) {
					root = root->left;
					dis++;
				}
				else if (A -> val > root -> val) {
					root = root->right;
					dis++;			
				}	
			}
			return dis;	
		}
	};
	
	
	int _tmain(int argc, _TCHAR* argv[])
	{
		Solution s;
		int a[] = {5, 6, 3, 1, 4, 2};
		int n1 = 2; 
		int n2 = 3;
		int n = 6;
		int *p = a;
		int dis = 0;
		TreeNode* root = s.buildBST(a, n);
		TreeNode* A = s.findNode(root, n1);
		TreeNode* B = s.findNode(root, n2);
		if (A == NULL || B == NULL) {
			dis =  -1;
		}
		TreeNode* C = s.findLCA(root, A, B);
		dis = s.calDis(root, A) + s.calDis(root, B) - 2 * s.calDis(root, C);
		return dis;
	}



