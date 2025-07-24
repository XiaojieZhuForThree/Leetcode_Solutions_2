#include <vector>
using std::pair;
using std::vector;

class Solution
{
public:
    vector<vector<int>> adj;
    int unxor(int a, int b)
    {
        return (a ^ b);
    }
    int precompute(int node, int restricted, vector<int> &precomputed, vector<int> &vis, vector<int> &nums)
    {
        int a = nums[node];
        vis[node] = 1;
        for (auto x : adj[node])
        {
            if (!vis[x] && x != restricted)
                a ^= precompute(x, restricted, precomputed, vis, nums);
        }
        precomputed[node] = a;
        return a;
    }
    void solve(int node, int root, int restricted, vector<int> &precomputed, vector<pair<int, int>> &vec, vector<int> &vis)
    {
        vis[node] = 1;
        for (auto x : adj[node])
        {
            if (!vis[x] && x != restricted)
            {
                int a = precomputed[root];
                int b = precomputed[x];
                int c = unxor(a, b);
                vec.push_back({c, b});
                solve(x, root, restricted, precomputed, vec, vis);
            }
        }
    }
    int firstcut(int root, vector<int> &vis, vector<int> &precomputed, vector<int> &nums)
    {
        int diff = INT_MAX;
        vis[root] = 1;
        int n = precomputed.size();
        for (auto x : adj[root])
        {
            if (!vis[x])
            {
                int a = precomputed[0];
                int b = precomputed[x];
                int c = unxor(a, b);
                vector<pair<int, int>> veci1, veci2;
                vector<int> vec1(n, 0), vec2(n, 0);
                vector<int> vis1(n, 0), vis2(n, 0);
                precompute(0, x, vec1, vis1, nums);
                precompute(x, root, vec2, vis2, nums);
                for (int i = 0; i < n; i++)
                {
                    vis1[i] = 0;
                    vis2[i] = 0;
                }
                solve(0, 0, x, vec1, veci1, vis1);
                solve(x, x, root, vec2, veci2, vis2);
                for (auto v : veci1)
                {
                    diff = std::min(diff, std::max(b, std::max(v.first, v.second)) - std::min(b, std::min(v.first, v.second)));
                }
                for (auto v : veci2)
                {
                    diff = std::min(diff, std::max(c, std::max(v.first, v.second)) - std::min(c, std::min(v.first, v.second)));
                }

                diff = std::min(diff, firstcut(x, vis, precomputed, nums));
            }
        }
        return diff;
    }

    int minimumScore(vector<int> &nums, vector<vector<int>> &edges)
    {
        int n = nums.size();
        adj.resize(n);
        for (auto edge : edges)
        {
            adj[edge[0]].push_back(edge[1]);
            adj[edge[1]].push_back(edge[0]);
        }

        vector<int> vis(n, 0);
        vector<int> precomputed(n, -1);
        precompute(0, -1, precomputed, vis, nums);
        for (int i = 0; i < n; i++)
            vis[i] = 0;
        return firstcut(0, vis, precomputed, nums);
        return 0;
    }
};
